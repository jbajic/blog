---
layout: post
title: Storage latencies every programmer should know
categories: [general]
---

Everyone knows about the [every latencies every programmer should know about](https://gist.github.com/jboner/2841832),
which contains latencies not only about storage devices but about mutex locks/unlocks, compression and other stuff. Here
is the data from that old GitHub gist from 2012":"

| Storage | Latency |
| --- | --- |
| L1 cache | 0.5 ns |
| L2 cache | 7 ns |
| RAM | 100 ns |
| Read SSD single block 4 KB | 150 000 ns = 150 μs |
| HDD disk seek | 10 000 000 ns = 20 000 μs = 20 ms |
| HDD sequential read | 20 000 000 ns = 150 000 μs = 150 ms |

This is a table of latencies from 2012, lets see how much the numbers have changed in eleven years and what additional
storage devices can we add here.

## CPU caches

L1 cache speed has not changed, but looking at real CPU values from [Intel](https://www.intel.com/content/www/us/en/developer/articles/technical/memory-performance-in-a-nutshell.htmlhttps://www.intel.com/content/www/us/en/developer/articles/technical/memory-performance-in-a-nutshell.html) we can see latencies quoted at 1 ns, and
L2 at 4 ns. But what changed is the size of CPU caches, L1 and L2 have become way bigger, e.g. in consumer products L1 cache increased form
4 KB to 32 KB, and for L2 from 32 KB to 256 KB.

Besides L1 and L2 caches, some CPUs have L3 caches. L3 being 10x slower then L2, coming to somewhere around 40 ns.
The major difference between L1, L2 and L3 caches is that L3 is shared among CPU cores, while every core has dedicated 
L1 and L2 cache, and also L3 cache size is around 8 MB.

## RAM

The main memory I am referring here is DRAM, and that latency number has not changed at all for eleven years, what changed were bandwidth and
capacity. The issue with improving [latency of DRAM](https://users.ece.cmu.edu/~omutlu/pub/understanding-latency-variation-in-DRAM-chips_sigmetrics16.pdf) is because of the big
variability in latencies when reading from DRAM. Reading from DRAM is broken into three phases (activation, precharge and restoration) and
all three phases can have different variable latencies.

# SSD

The SSD latencies performance can greatly vary which one we take into account. For example looking at Samsung Z-NAND SSD random
read latency is 12-20 μs. But looking at some other SSD also from Samsung (Samsung 870 QVO) whose latency is 1.7 ms

