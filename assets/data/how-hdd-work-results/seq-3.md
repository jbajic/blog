## For disk 3(ST4000VN006-3CW104_WW64VQYC - 4 TB (sde))
```
Starting 1 process
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [R(1)] [100.0% done] [494.1MB/0KB/0KB /s] [127K/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=20: Sun Apr  6 19:04:46 2025
  read : io=431739MB, bw=491222KB/s, iops=122805, runt=900001msec
    slat (usec): min=5, max=23001, avg= 7.18, stdev= 2.47
    clat (usec): min=10, max=90448, avg=122.80, stdev=56.19
     lat (usec): min=18, max=90454, avg=130.03, stdev=56.24
    clat percentiles (usec):
     |  1.00th=[  111],  5.00th=[  114], 10.00th=[  116], 20.00th=[  117],
     | 30.00th=[  118], 40.00th=[  119], 50.00th=[  120], 60.00th=[  121],
     | 70.00th=[  122], 80.00th=[  124], 90.00th=[  126], 95.00th=[  129],
     | 99.00th=[  139], 99.50th=[  454], 99.90th=[  498], 99.95th=[  516],
     | 99.99th=[ 1480]
    bw (KB  /s): min=152728, max=532056, per=100.00%, avg=491208.92, stdev=51285.58
    lat (usec) : 20=0.01%, 50=0.01%, 100=0.01%, 250=99.44%, 500=0.46%
    lat (usec) : 750=0.07%, 1000=0.01%
    lat (msec) : 2=0.03%, 4=0.01%, 10=0.01%, 20=0.01%, 50=0.01%
    lat (msec) : 100=0.01%
  cpu          : usr=8.40%, sys=89.80%, ctx=55798, majf=0, minf=25
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=110525114/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=431739MB, aggrb=491222KB/s, minb=491222KB/s, maxb=491222KB/s, mint=900001msec, maxt=900001msec
```