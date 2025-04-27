

## For disk 2(ST4000VN006-3CW104_WW64VQVG - 4 TB (sdc)):
```
Starting 1 process
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [R(1)] [100.0% done] [508.8MB/0KB/0KB /s] [130K/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=20: Sun Apr  6 18:42:45 2025
  read : io=441519MB, bw=502350KB/s, iops=125587, runt=900001msec
    slat (usec): min=5, max=356, avg= 7.03, stdev= 0.98
    clat (usec): min=9, max=45229, avg=120.08, stdev=83.00
     lat (usec): min=17, max=45236, avg=127.16, stdev=82.99
    clat percentiles (usec):
     |  1.00th=[  108],  5.00th=[  111], 10.00th=[  113], 20.00th=[  115],
     | 30.00th=[  116], 40.00th=[  117], 50.00th=[  118], 60.00th=[  119],
     | 70.00th=[  120], 80.00th=[  121], 90.00th=[  124], 95.00th=[  126],
     | 99.00th=[  133], 99.50th=[  173], 99.90th=[  494], 99.95th=[  516],
     | 99.99th=[ 1464]
    bw (KB  /s): min=139816, max=549520, per=99.99%, avg=502303.81, stdev=51861.89
    lat (usec) : 10=0.01%, 20=0.01%, 50=0.01%, 100=0.01%, 250=99.53%
    lat (usec) : 500=0.37%, 750=0.06%, 1000=0.01%
    lat (msec) : 2=0.03%, 4=0.01%, 10=0.01%, 20=0.01%, 50=0.01%
  cpu          : usr=8.37%, sys=89.98%, ctx=45150, majf=0, minf=25
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=113028898/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=441519MB, aggrb=502350KB/s, minb=502350KB/s, maxb=502350KB/s, mint=900001msec, maxt=900001msec
```