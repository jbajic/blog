## For disk 1(ST4000VN006-3CW104_WW64VAD1 - 4 TB (sdb)):
```
Starting 1 process
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [R(1)] [100.0% done] [473.1MB/0KB/0KB /s] [121K/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=20: Sun Apr  6 17:50:57 2025
  read : io=422967MB, bw=481242KB/s, iops=120310, runt=900001msec
    slat (usec): min=5, max=1019, avg= 7.29, stdev= 1.03
    clat (usec): min=9, max=113763, avg=125.37, stdev=87.56
     lat (usec): min=16, max=113771, avg=132.72, stdev=87.55
    clat percentiles (usec):
     |  1.00th=[  111],  5.00th=[  114], 10.00th=[  116], 20.00th=[  118],
     | 30.00th=[  119], 40.00th=[  121], 50.00th=[  122], 60.00th=[  124],
     | 70.00th=[  126], 80.00th=[  129], 90.00th=[  131], 95.00th=[  133],
     | 99.00th=[  143], 99.50th=[  462], 99.90th=[  502], 99.95th=[  524],
     | 99.99th=[ 1480]
    bw (KB  /s): min=74720, max=535920, per=100.00%, avg=481230.21, stdev=53032.47
    lat (usec) : 10=0.01%, 20=0.01%, 50=0.01%, 100=0.01%, 250=99.41%
    lat (usec) : 500=0.48%, 750=0.07%, 1000=0.01%
    lat (msec) : 2=0.03%, 4=0.01%, 10=0.01%, 20=0.01%, 50=0.01%
    lat (msec) : 100=0.01%, 250=0.01%
  cpu          : usr=8.70%, sys=89.38%, ctx=62722, majf=0, minf=26
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=108279522/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=422967MB, aggrb=481241KB/s, minb=481241KB/s, maxb=481241KB/s, mint=900001msec, maxt=900001msec
```
