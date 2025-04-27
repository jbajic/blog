## For disk 1(ST4000VN006-3CW104_WW64VAD1 - 4 TB (sdb)):
```
file1: (groupid=0, jobs=1): err= 0: pid=28: Sun Apr 27 16:57:08 2025
  read : io=995992KB, bw=1106.6KB/s, iops=276, runt=900113msec
    slat (usec): min=4, max=22482, avg=19.27, stdev=46.42
    clat (usec): min=164, max=1005.1K, avg=57814.74, stdev=63585.09
     lat (usec): min=170, max=1005.1K, avg=57834.32, stdev=63585.03
    clat percentiles (msec):
     |  1.00th=[    6],  5.00th=[    8], 10.00th=[   10], 20.00th=[   15],
     | 30.00th=[   20], 40.00th=[   27], 50.00th=[   36], 60.00th=[   48],
     | 70.00th=[   65], 80.00th=[   90], 90.00th=[  135], 95.00th=[  184],
     | 99.00th=[  306], 99.50th=[  363], 99.90th=[  494], 99.95th=[  545],
     | 99.99th=[  717]
    bw (KB  /s): min=  660, max= 1296, per=100.00%, avg=1107.30, stdev=75.32
    lat (usec) : 250=0.01%, 500=0.01%, 750=0.01%, 1000=0.01%
    lat (msec) : 2=0.01%, 4=0.15%, 10=11.51%, 20=19.29%, 50=30.83%
    lat (msec) : 100=21.19%, 250=14.95%, 500=1.98%, 750=0.09%, 1000=0.01%
    lat (msec) : 2000=0.01%
  cpu          : usr=0.31%, sys=0.80%, ctx=251077, majf=0, minf=27
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=248998/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16
file1: (groupid=1, jobs=1): err= 0: pid=29: Sun Apr 27 16:57:08 2025
  read : io=991.38MB, bw=1127.9KB/s, iops=281, runt=900080msec
    slat (usec): min=4, max=2834, avg=17.90, stdev=11.98
    clat (usec): min=329, max=1083.5K, avg=56722.46, stdev=62632.63
     lat (usec): min=342, max=1083.5K, avg=56740.62, stdev=62632.55
    clat percentiles (msec):
     |  1.00th=[    6],  5.00th=[    7], 10.00th=[   10], 20.00th=[   14],
     | 30.00th=[   19], 40.00th=[   26], 50.00th=[   36], 60.00th=[   47],
     | 70.00th=[   63], 80.00th=[   88], 90.00th=[  133], 95.00th=[  180],
     | 99.00th=[  302], 99.50th=[  359], 99.90th=[  494], 99.95th=[  545],
     | 99.99th=[  693]
    bw (KB  /s): min=  612, max= 1290, per=100.00%, avg=1128.55, stdev=70.89
    lat (usec) : 500=0.01%, 750=0.01%, 1000=0.01%
    lat (msec) : 4=0.19%, 10=11.95%, 20=19.45%, 50=30.86%, 100=21.05%
    lat (msec) : 250=14.57%, 500=1.84%, 750=0.09%, 1000=0.01%, 2000=0.01%
  cpu          : usr=0.29%, sys=0.76%, ctx=256027, majf=0, minf=26
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=253790/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=995992KB, aggrb=1106KB/s, minb=1106KB/s, maxb=1106KB/s, mint=900113msec, maxt=900113msec

Run status group 1 (all jobs):
   READ: io=991.38MB, aggrb=1127KB/s, minb=1127KB/s, maxb=1127KB/s, mint=900080msec, maxt=900080msec

Disk stats (read/write):
  md2p1: ios=0/0, merge=0/0, ticks=0/0, in_queue=0, util=0.00%
```