---
layout: post
title: Transaction Isolation 
categories: [database, concurrency]
---

**Transaction** is an abstraction used to emulate atomic execution of set of commands, and how well it
emulates depends on the isolation level. Since database systems support concurrent execution 
of multiple transactions, transaction levels serve as a way to isolate execution of one transaction 
from another in a specific way.

## Transaction execution

The issue that transaction levels are addressing is how *strict* we want to define transaction
execution. The most correct execution would be serial execution of every transaction in the 
in the order in which they arrive. Easiest way to do is to just execute transactions
in a single thread and viola. The obvious downside of that is that we are not utilizing the 
multicore architecture and possibility of parallel execution and also that we are waiting on IO.

It becomes intuitive to think that the more the stricter the isolation level the more expensive
execution becomes in the terms of:
 - **Transaction success**: more stricter control more transactions will be in conflict and get aborted,
 - **Slower execution**: in order to do all the checks or to hold the locks, the system
    will inevitably be slower.

But also lower the isolation level used more anomalies are possible.

## Isolation levels 

There are different levels of isolation and they are defined by the anomalies that can
occur under a specific isolation level. The table below shows four isolation levels; 
**Read Uncommitted**, **Read Committed**, **Repeatable Read** and **Serializable** and
which anomalies can happen in those levels.

Interestingly even though the lower isolation level is turned on, that only means
that certain anomalies might happen, but e.g. in Postgres it is not possible because
how the isolation is implemented using MVCC(multi version cuncurrency control). Even if 
the Read Uncommitted level is used, you still won't be able to read uncommitted changes.
Checkout more about this on [Postgres documentation](https://www.postgresql.org/docs/current/transaction-iso.html).

|   | Dirty Read | Nonrepeatable Read | Phantom Read | 
| --- | --- | --- | --- |
| **Read Uncommited** | Possible | Possible | Possible |
| **Read Committed** | Not Possible | Possible | Possible |
| **Repetable Read** | Not Possible | Not possible | Possible |
| **Serializable** | Not Possible | Not Possbile | Not Possible |

## Anomalies

Lets first see how these anomalies look like and how they affect database users. All the conflicting
operations which define anomalies have these two things in common:
 - They are from **different** transactions and
 - They operate on the same object and one of them is write.

In the following examples I will use transaction T1 and T2 to show the flow and how anomalies appear.
First column represents the order of execution of certain commands in transaction, T1 and T2 columns are
what transaction are doing and the State column represent the current database State.

### Dirty read

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN | | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | write(A: A - 10) |  | A<sub>T1</sub> = 90 |
| 3 | | read(A) = 90 | A = 100, A<sub>T1</sub> = 90|

In this anomaly transaction T2 has read the value that T1 has updated before T1 has committed, so
the transaction T2 has read changes that were never applied to the database. The issue with reading
uncommitted changes is that transaction that made them could abort, or it might not even be the final
changes that the transaction has made. Dirty reads are allowed in Read Uncommitted level.

Definition: **Dirty read** anomaly happens when one transaction reads the uncommitted changes of another transaction.

### Nonrepeatable Read

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN |  | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | read(A) = 100 |  | A = 100 |
| 3 | | write(A: 50) | A = 100, A<sub>T2</sub> = 50 |
| 4 |  | COMMIT | A = 50 |
| 5 | read(A) = 50 | | A = 50 |

In this scenario we are reading only committed values but in the scope of a single transaction
the value `A` got changed from `100` to `50`, without T1 transaction doing anything. This makes
the transaction not run in full isolation. This anomaly is sometimes referred as **Fuzzy Read**.

Definition: **Nonrepeatable read** anomaly happens when one transaction reads the same object 
multiple times and gets a different value for it, without making any updates to it.

### Phantom read

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN |  | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | data := read(x > 50) = ({A: 100})| | A = 100 |
| 3 | | write(B: 200) | A = 100, B<sub>T2</sub> = 200 |
| 4 |  | COMMIT | A = 100, B = 200 |
| 5 | data := read(x > 50) = ({A: 100}, {B: 200}) | | A = 100, B = 200 |
| 6 | COMMIT | | A = 100, B = 200, C = 300 |

In phantom read anomaly the issue was not in the particular object, but in the change of predicate result,
the result for query `x > 50` was changed for T1, and since there was no direct conflict,
the transaction did not truly run in a isolation, since the data that T1 saw changed before T1 could
commit.

Definition: **Phantom read** anomaly happens when one transaction predicate (select query) changes by having more
rows when the predicate is repeated.

## Additional anomalies

Besides these anomalies defined in SQL standard, there are others like: **Dirty write**, **Lost update**, **Read skew**
and **Write skew**, but they are not defined in SQL standard and it is not really clear at which level
these anomalies are possible. This makes the standard definition of isolation levels incomplete and allows 
different behavior for same isolation levels which has been critiqued for a long time.

### Dirty Write

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN |  | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | write(A: 200) | | A<sub>T1</sub> = 200, A = 100 |
| 3 | | write(A: 300) | A<sub>T1</sub> = 200, A<sub>T2</sub> = 300, A = 100 |
| 4 | COMMIT | | A<sub>T2</sub> = 300, A = 200 |
| 5 | | COMMIT | A = 300 |

Here T1 writes a value 200 to A, and T2 writes 300 to A, which is in a direct conflict where both
transactions want to write on the same object.

Definition: **Dirty write** happens when two transaction are trying to update the same object.

### Lost update

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN |  | A = 100 |
| 1 | | BEGIN | A = 100 |
| 2 | a = read(A) = 100 | | A = 100 |
| 3 |  | a = read(A) = 100 | A = 100 |
| 4 | write(A: 150) | | A<sub>T1</sub> = 150, A = 100 |
| 5 | COMMIT | | A = 150 |
| 6 | | write(A: 200) | A<sub>T2</sub> = 200, A = 150 |
| 7 | | COMMIT | A = 200 |

Lost update anomaly might seem very similar to the dirty write, but the issue here arises from the expectation
of the read value. First T1 reads A and then T2, and T1 updates A and commits, and transaction T2 still continues
executing under the assumption that the value it read `A = 100` is still true, and updates A to 200. The issue here
is that the update from T1 "disappears".

Definition: **Lost update** happens when multiple transaction are reading and updating the value, but the last update wins.
The difference between dirty read is that write of one transaction happens after the other commits.

### Read Skew

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN |  | A = 100, B = 200 |
| 1 | | BEGIN | A = 100, B = 200 |
| 2 | a = read(A) = 100 | | A = 100, B = 200 |
| 3 |  | write(B: 500) | A = 100, B = 200, B<sub>T2</sub> = 500 |
| 4 |  | COMMIT | A = 100, B = 500 |
| 5 | b = read(B) = 500 | | A = 100, B = 500 |
| 6 | write(C: a + b) | | A = 100, B = 500, C<sub>T1</sub> = 600 |
| 7 | COMMIT | | A = 100, B = 500, C = 600 |

In this anomaly T1 reads value `A` which is 100 and T2 updates `B` to 500 and commits, after that transaction
T1 reads B with newly committed data. Even though the data it read is valid it is not consistent with T1 being
executed in isolated environment.

Definition: **Read skew** is an expanded definition of phantom read, in phantom read we assumes only that insert may change the predicate
data, but here every inconsistency on the data we have read is considered anomaly.

### Write Skew

|  | T1 | T2 | State |
| --- | --- | --- | --- |
| 0 | BEGIN |  | A = 100, B = 200 |
| 1 | | BEGIN | A = 100, B = 200 |
| 2 | a = read(A) | | A = 100, B = 200 |
| 3 | | b = read(B) | A = 100, B = 200 |
| 4 | write(B: a) | | A = 100, B = 200, B<sub>T1</sub> = 100 |
| 5 | | write(A: b) | A = 100, B = 200, B<sub>T1</sub> = 100, A<sub>T2</sub> = 200 |
| 6 | COMMIT | | A = 100, B = 100, A<sub>T2</sub> = 200 |
| 7 | | COMMIT | A = 200, B = 100 |

In this scenario T1 reads value A, and T2 reads value B. After that T1 writes the value it read from A to B, and T2
does the opposite, it writes value A to B. After that T1 commits, and then T2 commits, and as a final results of both
transactions the values A and B have exchanged. But in serial execution of the transaction that could not happen.

Definition: **Write skew** is an anomaly in which transaction execution when writing data does not result in final
changes being executed in serial order.

## Issues with isolation levels & anomalies

Besides being incomplete the definition of levels are also ambiguous:
- Dirty Read does not define is the anomaly present in all four possible cases when T1 and T2 aborts and commits.
  (e.g. (T1: abort, T2: abort), (T1: commit, T2: abort), (T1: abort, T2: commit), (T1: commit, T2: commit))
- Nonrepeatable Read and Phantom Read anomalies are defined when inserting data but not when updating and deleting.
The anomalies are specific cases of non serializable schedule execution and sometimes they are implementation
specific, meaning in some implementation os isolation like MVCC, there are no lock and dirty reads and unrepetable
reads anomalies are not possible.

Also importantly the **Serializable** level is **NOT** a serializable transaction execution. Serializable level
is susceptible to write and read skew anomalies.

Unfortunately the standard does not define perfect isolation level, one that would lead to serializable transaction
execution, but a lot of database vendors have abandoned standard in this regard. So what is on standard defined
as Serializable isolation level some call **Snapshot Isolation**.

Alternative to defining isolation levels using anomalies is to define them using the state that the client would see as
the paper [Seeing is Believing](https://www.cs.cornell.edu/lorenzo/papers/Crooks17Seeing.pdf) speaks about.

# References
- Database System Concepts 7th edition, Avi Silberschatz, Henry F. Korth, S. Sudarshan 
- [A Critique of ANSI SQL Isolation Levels](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tr-95-51.pdf), Hal Berenson, Phil Bernstein, Jim Gray, Jim Melton, Elizabeth O’Neil, Patrick O'Neil
- [Seeing is Believing: A Client-Centric Specification of Database Isolation](https://www.cs.cornell.edu/lorenzo/papers/Crooks17Seeing.pdf), Natacha Crooks, Youer Pu,
  Lorenzo Alvisi, Allen Clement

