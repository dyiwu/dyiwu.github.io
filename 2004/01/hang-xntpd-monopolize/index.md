# Time services process xnptd monopolize CPU 0 in kernel mode.

## Time services process xnptd monopolize CPU 0 in kernel mode.

```

System crash dump analysis report
=================================
Symptom
-------
  System behaved as partial hang, no response on telnet/login.
  TOC dump be taken for root cause finding.

Root Cause
----------
  Time services process xnptd monopolize CPU 0 in kernel mode.

Action Plan
-----------
  Please install NTP timeservices patch PHNE_27223.

Detail Analysis
---------------
			=======================
			= General Information =
			=======================

Dump time Tue Jan 13 15:51:00 2004 UTC-8
System has been up 130 days, 1 hour, 56 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/N4000-55
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : PCXW+ (550 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 18.50  18.44  18.08  

			================
			= Crash Events =
			================

Crash event 0 was a TOC !
This seems to be a user initiated TOC !

Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x7edb58
= Using pc from pim.wide.rp_pcoq_head_hi = 0x24a080
==============  EVENT  ============================
SR5=0x00435c00
                SP         RP Return Name
0x400003ffffff1200 0x0024a080 soo_select2+0x360
0x400003ffffff1180 0x0013a7ac soo_select+0x14
0x400003ffffff1130 0x0010680c select+0xec
0x400003ffffff0dc0 0x00154940 syscall+0x608
0x400003ffffff0c70 0x00035944 syscallinit+0x54c


Stack Traces for other processors 
=================================


Processor #1
==============  EVENT  ============================
= Event #1 is TOC on CPU #1
= p crash_event_t 0x22030
= p rpb_t 0xf4b370
= Using pc from pim.wide.rp_pcoq_head_hi = 0x127af0
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000000d9d42b0 0x00127af0 idle+0x10b0
0x000000000d9d4050 0x0012a1f4 swidle+0x20

Processor #2
==============  EVENT  ============================
= Event #3 is TOC on CPU #2
= p crash_event_t 0x22090
= p rpb_t 0xf4ba50
= Using pc from pim.wide.rp_pcoq_head_hi = 0x127a88
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000000d9d72b0 0x00127a88 idle+0x1048
0x000000000d9d7050 0x0012a1f4 swidle+0x20

Processor #3
==============  EVENT  ============================
= Event #2 is TOC on CPU #3
= p crash_event_t 0x22060
= p rpb_t 0xf4c130
= Using pc from pim.wide.rp_pcoq_head_hi = 0x1277a4
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000000d9da2b0 0x001277a4 idle+0xd64
0x000000000d9da050 0x0012a1f4 swidle+0x20


			==================
			= Memory Globals =
			==================

Physical Memory     = 1048576 pages (4.00 GB)
Free Memory         = 759474 pages (2.90 GB)
Average Free Memory = 759196 pages (2.90 GB)
gpgslim             = 3072 pages (12.00 MB)
lotsfree            = 16384 pages (64.00 MB)
desfree             = 3072 pages (12.00 MB)
minfree             = 1280 pages (5.00 MB)

			========================
			= Buffer Cache Globals =
			========================

dbc_max_pct           = 20 %
dbc_min_pct           = 5 %
dbc current pct       = 5.7 %
bufpages              = 60000 pages (234.38 MB)
Number of buf headers = 43113

fixed_size_cache = 1 
dbc_parolemem    = 0 
dbc_stealavg     = 0 
dbc_ceiling      = 60000 pages (234.38 MB)
dbc_nbuf         = 30000 
dbc_bufpages     = 60000 pages (234.38 MB)
dbc_vhandcredit  = 17757555 
orignbuf         = 30000 
origbufpages     = 60000 pages (234.38 MB)

			====================
			= Swap Information =
			====================

swapinfo -mt emulation
======================

             Mb      Mb      Mb   PCT  START/      Mb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev        1024       0    1024    0%       0       -    1  LVM vg00/lv2
reserve       -     430    -430
memory     3119     461    2658   15%
total      4143     891    3252   22%       -       0    -

			======================
			= Network Interfaces =
			======================

Name   PPA   Driver      Interface           Mac       States       IP
              Name      Description        Address    Link IP     Address
--------------------------------------------------------------------------------
lan0    0    btlan3  100BT PCI Built-in 0x001083ff2bc6 UP   UP   172.18.41.187
lan2    2    btlan5  100BT PCI Add-on   0x00306e05fc84 UP   DOWN 0.0.0.0      
lan3    3    btlan6  100BT PCI          0x0060b07a321b UP   UP   192.6.1.1    



If you want more information, you can use : "lanshow -f"

			====================
			= IOVA Usage Check =
			====================


98% of IOVA still available/free.

		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 4


        s
        t
        a       spin reg eiem/spl         eirr             ipsw            
evt cpu t type  dpth src cr15             cr23             cr22            
--- --- - ----- ---- --- ---------------- ---------------- ----------------
0   0   E TOC   0    rpb fffffff0ffffffff 0000000000000080 0804001f WCRQPDI
1   1   E TOC   0    rpb fffffff0ffffffff 0000000000000400 0804fe1f WCRQPDI
2   3   E TOC   0    rpb fffffff0ffffffff 0000000000000000 0804fc1f WCRQPDI
3   2   E TOC   0    rpb fffffff0ffffffff 0000000000000000 0804fc1f WCRQPDI

			=================
			= Load Averages =
			=================


avenrun
=======
18.50  18.44  18.08  

real_run
========
74.003543  73.742716  72.250179  

pwrun ("fast" io wait)
======================
0.000002  0.019487  0.070459  

mp_avenrun
==========
cpu0 : 73.998070  73.668383  72.131284  <--- hevay loaded
cpu1 : 0.000016  0.053023  0.115007  
cpu2 : 0.002812  0.020418  0.033760  
cpu3 : 0.002647  0.020379  0.040586  

Running Threads (TSRUNPROC) and idle Processors
===============================================


                    TICKS      TICKS                    I TICKS
                    SINCE      SINCE                    C SINCE      NREADY
TID     PID   PPID  RUN        IDLE       PRI SPU STATE S MIGR       FR LO AL COMMAND
------- ----- ----- ---------- ---------- --- --- ----- - ---------  -- -- -- -------
20955   20031 1     34076060   34076060   120 0   SYS   N 34076060   0  86 0  xntpd 
                               0              1   IDLE  N 310        0  0  0  
                               0              2   IDLE  N 112        0  0  0  
                               0              3   IDLE  N 212        0  0  0  
JAGae26628

```

