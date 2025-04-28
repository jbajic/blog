## For disk 1(ST4000VN006-3CW104_WW64VAD1 - 4 TB (sdb)):
```
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [r(1)] [100.0% done] [1228KB/0KB/0KB /s] [307/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=27: Mon Apr 28 19:56:31 2025
  read : io=985.82MB, bw=1121.6KB/s, iops=280, runt=900093msec
    slat (usec): min=3, max=341, avg=18.29, stdev= 9.11
    clat (usec): min=201, max=1035.4K, avg=57043.45, stdev=63589.64
     lat (usec): min=212, max=1035.4K, avg=57062.03, stdev=63589.57
    clat percentiles (msec):
     |  1.00th=[    5],  5.00th=[    7], 10.00th=[    9], 20.00th=[   14],
     | 30.00th=[   19], 40.00th=[   26], 50.00th=[   35], 60.00th=[   47],
     | 70.00th=[   64], 80.00th=[   89], 90.00th=[  135], 95.00th=[  182],
     | 99.00th=[  306], 99.50th=[  363], 99.90th=[  498], 99.95th=[  553],
     | 99.99th=[  709]
    bw (KB  /s): min=  444, max= 1368, per=100.00%, avg=1122.50, stdev=103.99
    lat (usec) : 250=0.01%, 750=0.01%, 1000=0.02%
    lat (msec) : 2=0.01%, 4=0.21%, 10=12.45%, 20=19.36%, 50=30.39%
    lat (msec) : 100=20.78%, 250=14.77%, 500=1.91%, 750=0.09%, 1000=0.01%
    lat (msec) : 2000=0.01%
  cpu          : usr=0.28%, sys=0.81%, ctx=254591, majf=0, minf=27
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=252363/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=985.82MB, aggrb=1121KB/s, minb=1121KB/s, maxb=1121KB/s, mint=900093msec, maxt=900093msec

Disk stats (read/write):
  md1p1: ios=0/0, merge=0/0, ticks=0/0, in_queue=0, util=0.00%
```
