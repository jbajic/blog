## For disk 3(ST4000VN006-3CW104_WW64VQYC - 4 TB (sde))
Starting 1 process
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [r(1)] [100.0% done] [1162KB/0KB/0KB /s] [290/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=26: Sun Apr 27 17:24:45 2025
  read : io=998928KB, bw=1109.9KB/s, iops=277, runt=900076msec
    slat (usec): min=4, max=3098, avg=19.69, stdev=14.00
    clat (usec): min=200, max=915661, avg=57641.95, stdev=63896.23
     lat (usec): min=208, max=915693, avg=57661.96, stdev=63896.14
    clat percentiles (msec):
     |  1.00th=[    6],  5.00th=[    8], 10.00th=[   10], 20.00th=[   14],
     | 30.00th=[   20], 40.00th=[   27], 50.00th=[   36], 60.00th=[   48],
     | 70.00th=[   64], 80.00th=[   89], 90.00th=[  135], 95.00th=[  184],
     | 99.00th=[  310], 99.50th=[  367], 99.90th=[  502], 99.95th=[  562],
     | 99.99th=[  676]
    bw (KB  /s): min=  656, max= 1301, per=100.00%, avg=1110.51, stdev=74.46
    lat (usec) : 250=0.01%, 500=0.01%, 750=0.01%, 1000=0.01%
    lat (msec) : 2=0.01%, 4=0.15%, 10=11.73%, 20=19.24%, 50=30.84%
    lat (msec) : 100=21.17%, 250=14.75%, 500=1.98%, 750=0.10%, 1000=0.01%
  cpu          : usr=0.31%, sys=0.83%, ctx=251990, majf=0, minf=26
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=249732/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=998928KB, aggrb=1109KB/s, minb=1109KB/s, maxb=1109KB/s, mint=900076msec, maxt=900076msec

Disk stats (read/write):
  md3p1: ios=0/0, merge=0/0, ticks=0/0, in_queue=0, util=0.00%