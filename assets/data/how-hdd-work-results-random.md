## For disk 1(ST4000VN006-3CW104_WW64VAD1 - 4 TB (sdb)):
```
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [r(1)] [100.0% done] [1180KB/0KB/0KB /s] [295/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=20: Sun Apr  6 20:37:57 2025
  read : io=984956KB, bw=1094.3KB/s, iops=273, runt=900088msec
    slat (usec): min=6, max=59926, avg=21.42, stdev=121.34
    clat (usec): min=171, max=346527, avg=58460.85, stdev=21107.99
     lat (usec): min=182, max=346536, avg=58482.48, stdev=21107.69
    clat percentiles (msec):
     |  1.00th=[   17],  5.00th=[   28], 10.00th=[   34], 20.00th=[   42],
     | 30.00th=[   47], 40.00th=[   52], 50.00th=[   58], 60.00th=[   63],
     | 70.00th=[   69], 80.00th=[   76], 90.00th=[   85], 95.00th=[   93],
     | 99.00th=[  114], 99.50th=[  127], 99.90th=[  180], 99.95th=[  204],
     | 99.99th=[  273]
    bw (KB  /s): min=  547, max= 1727, per=100.00%, avg=1094.98, stdev=162.23
    lat (usec) : 250=0.01%, 500=0.01%
    lat (msec) : 4=0.02%, 10=0.22%, 20=1.50%, 50=34.08%, 100=61.38%
    lat (msec) : 250=2.77%, 500=0.02%
  cpu          : usr=0.16%, sys=0.69%, ctx=112614, majf=0, minf=25
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=246239/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=984956KB, aggrb=1094KB/s, minb=1094KB/s, maxb=1094KB/s, mint=900088msec, maxt=900088msec
```

## For disk 2(ST4000VN006-3CW104_WW64VQVG - 4 TB (sdc)):
```
fio-2.2.10
Starting 1 process
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [r(1)] [100.0% done] [1200KB/0KB/0KB /s] [300/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=20: Sun Apr  6 20:18:19 2025
  read : io=1074.5MB, bw=1222.5KB/s, iops=305, runt=900059msec
    slat (usec): min=6, max=361, avg=18.66, stdev=12.02
    clat (usec): min=133, max=323024, avg=52333.50, stdev=20223.23
     lat (usec): min=143, max=323033, avg=52352.34, stdev=20223.18
    clat percentiles (msec):
     |  1.00th=[   13],  5.00th=[   23], 10.00th=[   29], 20.00th=[   36],
     | 30.00th=[   42], 40.00th=[   47], 50.00th=[   51], 60.00th=[   57],
     | 70.00th=[   62], 80.00th=[   69], 90.00th=[   78], 95.00th=[   86],
     | 99.00th=[  104], 99.50th=[  118], 99.90th=[  174], 99.95th=[  194],
     | 99.99th=[  253]
    bw (KB  /s): min=  507, max= 1756, per=100.00%, avg=1223.32, stdev=154.50
    lat (usec) : 250=0.05%, 500=0.01%, 750=0.01%, 1000=0.01%
    lat (msec) : 2=0.01%, 4=0.04%, 10=0.44%, 20=2.95%, 50=44.48%
    lat (msec) : 100=50.64%, 250=1.36%, 500=0.01%
  cpu          : usr=0.16%, sys=0.67%, ctx=112201, majf=0, minf=25
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=275064/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=1074.5MB, aggrb=1222KB/s, minb=1222KB/s, maxb=1222KB/s, mint=900059msec, maxt=900059msec
```

## For disk 3(ST4000VN006-3CW104_WW64VQYC - 4 TB (sde))
```
fio-2.2.10
Starting 1 process
file1: Laying out IO file(s) (1 file(s) / 10240MB)
Jobs: 1 (f=1): [r(1)] [100.0% done] [1112KB/0KB/0KB /s] [278/0/0 iops] [eta 00m:00s]
file1: (groupid=0, jobs=1): err= 0: pid=20: Sun Apr  6 19:43:51 2025
  read : io=978.72MB, bw=1113.5KB/s, iops=278, runt=900055msec
    slat (usec): min=6, max=89242, avg=21.45, stdev=305.77
    clat (usec): min=156, max=353262, avg=57453.21, stdev=21281.86
     lat (usec): min=165, max=353271, avg=57474.85, stdev=21280.38
    clat percentiles (msec):
     |  1.00th=[   16],  5.00th=[   27], 10.00th=[   33], 20.00th=[   41],
     | 30.00th=[   46], 40.00th=[   51], 50.00th=[   57], 60.00th=[   62],
     | 70.00th=[   68], 80.00th=[   74], 90.00th=[   84], 95.00th=[   93],
     | 99.00th=[  116], 99.50th=[  131], 99.90th=[  184], 99.95th=[  212],
     | 99.99th=[  273]
    bw (KB  /s): min=  451, max= 1569, per=100.00%, avg=1114.30, stdev=150.03
    lat (usec) : 250=0.02%, 500=0.01%
    lat (msec) : 2=0.01%, 4=0.03%, 10=0.23%, 20=1.76%, 50=35.75%
    lat (msec) : 100=59.43%, 250=2.76%, 500=0.02%
  cpu          : usr=0.14%, sys=0.67%, ctx=111060, majf=0, minf=25
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=100.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.1%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=250548/w=0/d=0, short=r=0/w=0/d=0, drop=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=16

Run status group 0 (all jobs):
   READ: io=978.72MB, aggrb=1113KB/s, minb=1113KB/s, maxb=1113KB/s, mint=900055msec, maxt=900055msec
```