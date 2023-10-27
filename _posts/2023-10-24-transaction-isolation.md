---
title: Transaction Isolation
layout: post
categories:
- Database
- Concurrency 
---

**Transaction** is an abstraction used to emulate atomic execution of set of commands, and how well it
emulates depends on isolation level. Since database systems support concurrent execution 
of multiple transactions, transaction levels serve as a way to isolate execution of one transaction 
from another in a specific way.

## Transaction execution
The issue that transaction levels are addressing is how *strict* we want to define transaction
execution. The most correct execution would be serial execution of every transaction in the 
order in which they arrive. And to do that most easily would be just to execute transactions
in a single thread and viola. The obvious downside of that is that we are not utilizing the 
multicore architecture and possibility of parallel execution and also that we are waiting on IO.

It becomes intuitive to think that the more the stricter the isolation level the more expensive
execution becomes in the terms of:
 - **Transaction success**: more stricter control more transactions will get aborted,
 - **Slower execution**: in order to do all the check or hold all the locks, the systems
    will inevitably be slower

## Isolation levels 
There are different levels of isolation and they are defined by the anomalies that can
occur under specific isolation level. The table below shows four isolation levels; 
**Read Uncommitted**, **Read Committed**, **Repeatable Read** and **Serializable** and
which anomalies can happen in those levels.

Interestingly even though the lower isolation level is turned on, that only means
that certain anomalies might happened, but e.g. in Postgres it is not possible because
how the isolation is implemented using MVCC(multi version cuncurrency control). Even if 
the Read Uncommitted level is used, you still won't be able to read uncommitted changes.
Checkout more about this on [Postgres documentation](https://www.postgresql.org/docs/current/transaction-iso.html).

|   | Dirty Read | Nonrepeatable Read | Phantom Read | Serialization Anomaly |
| --- | --- | --- | --- | --- |
| **Read Uncommited** | Possible | Possible | Possible | Possible | 
| **Read Committed** | Not Possible | Possible | Possible | Possible | 
| **Repetable Read** | Not Possible | Not possible | Possible | Possible | 
| **Serializable** | Not Possible | Not Possbile | Not Possible |  Not Possible | 

## Anomalies
Lets first see how these anomalies look like and how they affect database users. All the conflicting
operations which define anomalies have these two things in common:
 - They are from **different** transactions and
 - They operate on the same object and one of them is write.

In the following examples I will use transaction T1 and T2 to show the flow and how anomalies appear.
Imagine we have transactions T1 and T2 like this and the starting value of `A` is `100`.

### Lost update

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN | | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | write(A: 1000) | | A<sub>T1</sub> = 1000 |
| 3 | | write(A: 2000) | A<sub>T1</sub> = 1000, A<sub>T2</sub> = 2000 |
| 4 | | COMMIT | A = 1000, A<sub>T2</sub> = 2000 |
| 5 | COMMIT | | A = 2000 |

This is not possible in either of isolation levels, since this is the most basic of write write 
conflicts that database should guard against.

### Dirty read

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN | | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | read(A) = 100 |  | A<sub>T1</sub> = 100 |
| 3 | write(A: A - 10) |  | A<sub>T1</sub> = 90 |
| 4 | | read(A) = 90 | A<sub>T1</sub> = 90, A<sub>T2</sub> = 90 |
| 5 | | write(A: A - 10) = 80 | A<sub>T1</sub> = 90, A<sub>T2</sub> = 80 | 
| 6 | ABORT | | A<sub>T2</sub> = 80 |
| 7 | | END | A = 80 |

In this case the T2 has read the value that T1 has update before T1 has committed, so in this case
the transaction T2 has read changes that were never applied to the database. The issue with reading
uncommitted changes is that transaction that made them could easily revert them, or they might not
event be the final changes that the transaction has made. Dirty reads are allowed in Read Uncommitted
level according to the SQL standard.

### Nonrepeatable Read

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN |  | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | read(A) = 100 |  | A<sub>T1</sub> = 100 |
| 3 |  | read(A) = 100 | A<sub>T1</sub> = 100, A<sub>T2</sub> = 100 |
| 4 | | write(A: A - 50) | A<sub>T1</sub> = 100, A<sub>T2</sub> = 50 |
| 5 |  | COMMIT | A<sub>T1</sub> = 100, A = 50 |
| 6 | read(A) = 50 | | A<sub>T1</sub> = 50, A = 50 |
| 7 | END | A = 50 |

In this scenario we are reading only committed values but in the scope of a single transaction
the value `A` got changed from `100` to `50`, without T1 transaction doing anything. This does not
cause issues with data integrity by itself but it makes transaction not run in full isolation.

### Phantom read

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN |  | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | data := read(x < 50) = ({A: 100})| | A = 100 |
| 3 | | write(B: 200) | A = 100, B<sub>T2</sub> = 200 |
| 4 |  | COMMIT | A = 100, B = 200 |
| 5 | IF data.size() == 1: write(C: 300) | | A = 100, B = 200, C<sub>T1</sub> = 300 |
| 6 | COMMIT | | A = 100, B = 200, C = 300 |

In phantom read anomaly the issue was not in the particular object, but in the change of predicate result,
the result for query `x < 50` has changed for T1 by T2, and since there was no direct conflict,
the transaction did not truly run in a isolation, since the data that T1 saw changed before T1 could
commit.

## Isolation levels
Isolation level are best defined by the anomalies they allow. SQL standard defines the levels shown
in the table above, but with various databases existing there are other isolation levels out there, like:
snapshot isolation.

## Conclusion
Understanding anomalies ensures we understand the isolation level database is operating under,
and that makes application developer life easier.
