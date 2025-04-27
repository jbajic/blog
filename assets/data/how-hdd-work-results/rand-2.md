## For disk 2(ST4000VN006-3CW104_WW64VQVG - 4 TB (sdc)):
```
file1: (g=0): rw=randread, bs=4K-4K/4K-4K/4K-4K, ioengine=libaio, iodepth=16
fio-2.2.10
Starting 1 process
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [r(1)] [100.0% done] [1000KB/0KB/0KB /s] [250/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=27: Sun Apr 27 16:11:59 2025
  read : io=990852KB, bw=1100.9KB/s, iops=275, runt=900088msec
    slat (usec): min=4, max=322, avg=16.15, stdev= 8.89
    clat (usec): min=156, max=1209.4K, avg=58117.20, stdev=65001.13
     lat (usec): min=161, max=1209.4K, avg=58133.60, stdev=65001.09
    clat percentiles (msec):
     |  1.00th=[    5],  5.00th=[    7], 10.00th=[    9], 20.00th=[   14],
     | 30.00th=[   19], 40.00th=[   26], 50.00th=[   36], 60.00th=[   48],
     | 70.00th=[   65], 80.00th=[   91], 90.00th=[  137], 95.00th=[  186],
     | 99.00th=[  310], 99.50th=[  367], 99.90th=[  506], 99.95th=[  578],
     | 99.99th=[  742]
    bw (KB  /s): min=  419, max= 1346, per=100.00%, avg=1102.00, stdev=108.71
    lat (usec) : 250=0.01%, 500=0.01%, 750=0.03%, 1000=0.05%
    lat (msec) : 2=0.01%, 4=0.25%, 10=12.33%, 20=19.01%, 50=30.27%
    lat (msec) : 100=20.72%, 250=15.18%, 500=2.04%, 750=0.10%, 1000=0.01%
    lat (msec) : 2000=0.01%
  cpu          : usr=0.24%, sys=0.72%, ctx=249379, majf=0, minf=26
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=247713/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=990852KB, aggrb=1100KB/s, minb=1100KB/s, maxb=1100KB/s, mint=900088msec, maxt=900088msec

Disk stats (read/write):
  md1p1: ios=0/0, merge=0/0, ticks=0/0, in_queue=0, util=0.00%
```