---
layout: post
title: How do HDDs affect database operations?
categories: [general, storage]
---

## How do Hard Disk Drives work?

HDD is a non volatile storage device which stores the data in a form of a magnetic
field. The HDD is made out of multiple magnetic (from one to five platters) **platters** and pair of arms below and above of each platter.
Each platter contains multiple **tracks** which is a circular path on the platter.
And tracks contains smaller chunks called **sectors**. Data is read and written
sector by sector using the arms. Sectors usually have sizes in range from 512 B
to 16 KB. And a single track can have [many sectors](https://superuser.com/questions/107723/hard-drive-sectors-vs-tracks)
depending how far away is track from the center of the platter. Modern HDD mostly
have 16 KB sectors. It is important mentioning that HDD devices guarantee that
the writes of sectors will happen atomically. Here is my humble hand drawn
representation if this:
![HDD internal hand drawn representation](/assets/image/hdd-internal.png)

Mechanism of writing and reading data is based on magnetic field of the platters. When disk arm generates current it changes the magnetic field of the platter,
which **writes** the data down. Detecting the magnetic field in arm generates the
current in arm and therefore reads the data. Newer HDDs have separate heads for
reading and writing the data.

A interesting question to ask is why not add multiple arms to increase HDD speed
and add internal parallelism like SSDs have? As said in this [answer]
(https://superuser.com/questions/1137805/why-arent-there-multiple-heads-covering-the-radius-of-a-hard-disk-platter)
the issue is that would introduce additional complexity of calculating and moving
the arms and would also create more heat within the HDDs, and besides that they
would be much more expensive for consumer grade products. But looking at the
existing HDDs there are some implementing multiple arms e.g. Seagate MACH.2

## Total latency

When the HDD needs to read the data few things have to happen first:
1. The OS provides the exact block/sector that needs to be read from the disk.
2. The HDD calculates on which track the sector is and on which platter.
3. If the arm is not at the position of the correct track, arm has to be repositioned,
this is called **seek time** and it ranges from 2 ms to 20 ms depending on how far
away they are.
4. After that the platter keeps rotating until it finds the correct sector this
is called **rotational latency time**. This time can range between 4 ms and 11 ms
per rotation.
Full access time is the sum of seek time and rotational latency time. In summary:

## Latency & bandwidth

Showing how HDDs work we have already covered their latency which can be expressed
in this formula:
```math
TotalLatency = SeekTime + RotationalLatencyTime
```
The latency of disk seek can greatly vary depending on if it is a sequential or
random access. Taking a look at [Latencies every programmer should know](https://gist.github.com/jboner/2841832) it matches or expectations around 20 ms.

But bandwidth of HDD can vary greatly from one manufacturer to other, but accepted
bandwidth rate for reading/writing is 30-150 MB/s. SATA cables are not a bottleneck, since SATA 3 transfer speed is around 6 Gbit/s which is 750 MB/s, more then HDD
bandwidth.

## Sequential vs Random access

Let us try to test out he difference between random and sequential access to
compare the speed and see the effect on the write/read operations. I will be using
fio to measure different workloads on a HDD. I am using: [Western Digital
WD10EZEX-60WN4A1 - 1TB 7.2K RPM SATA 3.5" Hard Drive HDD](https://smarthdd.com/database/WDC-WD10EZEX-60WN4A1/03.01A03/) which according to the
specification has:
- average seek time: 8ms,
- rotational speed 7200 RPM, which means it needs 8.33 ms to do a full rotation.
- maximum read speed of 210 MB/s and maximum buffered read speed of 425 MB/s

I am interested if the tests with fio will match the results in specifications, so
lets see.

## References
- https://en.wikipedia.org/wiki/Disk_read-and-write_head#:~:text=A%20disk%20read%2Dand%2Dwrite,magnetic%20field%20into%20electric%20current
- https://superuser.com/questions/107723/hard-drive-sectors-vs-tracks sectors in tracks
- Database System Concepts 7th Edition