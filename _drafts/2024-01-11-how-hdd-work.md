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
```math
TotalLatency = SeekTime + RotationalLatencyTime
```

## Sequential vs Random access

## References
- https://en.wikipedia.org/wiki/Disk_read-and-write_head#:~:text=A%20disk%20read%2Dand%2Dwrite,magnetic%20field%20into%20electric%20current
- https://superuser.com/questions/107723/hard-drive-sectors-vs-tracks sectors in tracks
- Database-System-Concepts-7th-Edition