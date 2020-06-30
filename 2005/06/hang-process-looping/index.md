# System hang with process looping in soaccept().

## System hang with process looping in soaccept().

```

            System crash dump analysis report
            =================================

Symptom
-------
  System behaved as hang. TOC dump be taken for root cuase finding.

Root Cause
----------
  Due to a race condition, process is looping in soaccept().
  A TOC show the following stack trace:
    Cpu 0  : proc[99] pid=9693 tid=11512  cmd="/opt/java1.4/bin/PA_RISC2.0/ja"
    Processor #0
    ==============  EVENT  ============================
    = Event #0 is TOC on CPU #0
    = p crash_event_t 0x22000
    = p rpb_t 0x7b7358
    = Using pc from pim.wide.rp_pcoq_head_hi = 0x3482c
    ==============  EVENT  ============================
    SR5=0x29a5400
                   SP     SZ        RP  FUNC
    0x400003ffffff1730    0x0   0x3482c  spinlock+0x2c
    0x400003ffffff1730  0x170   0xcad94  issig+0x8c
    0x400003ffffff15c0  0x130  0x12befc  _sleep+0x24c
    0x400003ffffff1490  0x100  0x11d09c  read_sleep+0x1ac
    0x400003ffffff1390  0x210  0x138660  streams_getmsg+0x1a0
    0x400003ffffff1180  0x260  0x139900  soaccept+0x9d8
    0x400003ffffff0f20   0xa0  0x136050  sodequeue+0xb8
    0x400003ffffff0e80   0xc0  0x13a1ec  accept+0x22c
    0x400003ffffff0dc0  0x150  0x1574a4  syscall+0x28c
    0x400003ffffff0c70  0x4d0   0x35944  syscallinit+0x54c


Action Plan
-----------
 The following patches are recommended.

  Catalog     Text      (17 patches 44MB)
  ----------  ---------------------------------------------
  PHCO_23651  fsck_vxfs(1M) cumulative patch
  PHCO_24437  LVM commands cumulative patch
  PHCO_28125  cumulative SAM/ObAM patch
  PHCO_29380  user/group(add/mod/del)(1M) cumulative patch
  PHKL_18543  PM/VM/UFS/async/scsi/io/DMAPI/JFS/perf patch
  PHKL_19202  fsadm panic if extending root on 11.x
  PHKL_20016  2nd CPU not recognized in G70/H70/I70
  PHKL_23409  NFS, Large Data Space, kernel memory leak
  PHKL_29693  VxFS 3.1 cumulative patch: CR_EIEM
  PHKL_30578  callout corruption/abstime callouts/sleep
  PHKL_30972  hang in aio_shutdown_fd_begin();EVP
  PHKL_31867  Probe,IDDS,PM,VM,PA-8700,AIO,T600,FS,PDC,CLK
  PHKL_32920  LVM Cumulative Patch
  PHNE_28525  Cumulative STREAMS Patch
  PHNE_29773  sendmail(1m) 8.9.3 patch
  PHNE_31965  ARPA transport commands and libnm patch
  PHNE_32041  cumulative ARPA Transport patch

Detail Analysis
---------------
Ref: 4000013114 JAGae18049 JAGae14249 JAGad26905 JAGad88349

p4>trace -v processor 0
Cpu 0  : proc[99] pid=9693 tid=11512  cmd="/opt/java1.4/bin/PA_RISC2.0/ja"
Processor #0
==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x7b7358
= Using pc from pim.wide.rp_pcoq_head_hi = 0x3482c
==============  EVENT  ============================
SR5=0x29a5400
LVL  FUNC                  ARG0                ARG1                ARG2                ARG3  ARG4  ARG5  ARG6  ARG7    
 0)  spinlock+0x2c         0x6bd5bc0           n/a                 n/a                 n/a   n/a   n/a   n/a   n/a     
 1)  issig+0x8c            0x2                 n/a                 n/a                 n/a   n/a   n/a   n/a   n/a     
 2)  _sleep+0x24c          0x4c11c468          0x1029a             n/a                 n/a   n/a   n/a   n/a   n/a     
 3)  read_sleep+0x1ac      0x4c11c500          n/a                 n/a                 n/a   n/a   n/a   n/a   n/a     
 4)  streams_getmsg+0x1a0  0x4c11c400          0x400003ffffff1118  0x1                 n/a   n/a   n/a   n/a   n/a     
 5)  soaccept+0x9d8        0x48389200          0x41d92600          
                        0x400003ffffff0ec0  n/a   n/a   n/a   n/a   n/a     
 6)  sodequeue+0xb8        0x48389200          0x400003ffffff0e18  
                       0x400003ffffff0e00  n/a   n/a   n/a   n/a   n/a     
 7)  accept+0x22c          0x400003ffffff03a0  n/a                 n/a                 n/a   n/a   n/a   n/a   0x331520
 8)  syscall+0x28c         n/a                 n/a                 n/a                 n/a   n/a   n/a   n/a   n/a     
 9)  syscallinit+0x54c     n/a                 n/a                 n/a                 n/a   n/a   n/a   n/a   n/a     

p4> Lock_t 0x06bd5bc0
0x06bd5bc0
0x06bd5bc0 :
0x06bd5bc0 lock_t {
0x06bd5bc0   u_int   sl_lock;            0x00000000
0x06bd5bc4   u_int   sl_owner;           0x00000000
0x06bd5bc8   u_int   sl_flag;            0x00000000
0x06bd5bcc   u_int   sl_next_cpu;        0x00000000
0x06bd5bd0   u_int   sl_indirect:0:1;    0x0
0x06bd5bd0   u_int   sl_name_ptr:1:23;   0x1a184c  ("thread lock")
0x06bd5bd4   u_int   sl_pad[3];          0x00000000
0x06bd5be0 };

p4> trace -v tid 11512
proc[99] pid=9693 tid=11512  cmd="/opt/java1.4/bin/PA_RISC2.0/java -Djboss"
Process  : p proc_t    0x35c0380
           proc[99] pid=9693 java
Kthread  : p kthread_t 0x3609300
           Running on SPU #0

Processor #0
==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x7b7358
= Using pc from pim.wide.rp_pcoq_head_hi = 0x3482c
==============  EVENT  ============================
SR5=0x29a5400
                SP     SZ        RP  FUNC
0x400003ffffff1730    0x0   0x3482c  spinlock+0x2c
0x400003ffffff1730  0x170   0xcad94  issig+0x8c
0x400003ffffff15c0  0x130  0x12befc  _sleep+0x24c
0x400003ffffff1490  0x100  0x11d09c  read_sleep+0x1ac
0x400003ffffff1390  0x210  0x138660  streams_getmsg+0x1a0
0x400003ffffff1180  0x260  0x139900  soaccept+0x9d8
0x400003ffffff0f20   0xa0  0x136050  sodequeue+0xb8
0x400003ffffff0e80   0xc0  0x13a1ec  accept+0x22c
0x400003ffffff0dc0  0x150  0x1574a4  syscall+0x28c
0x400003ffffff0c70  0x4d0   0x35944  syscallinit+0x54c

			=======================
			= General Information =
			=======================

Dump time Tue Jun  7 10:23:01 2005 UTC-8
System has been up 16 hours, 13 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/L3000-7x
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : PCXW+ (750 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 13.99  13.38  11.96  

			================
			= Crash Events =
			================


Note: Crash event 0 was a TOC !

Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x7b7358
= Using pc from pim.wide.rp_pcoq_head_hi = 0x3482c
==============  EVENT  ============================
SR5=0x029a5400
                SP         RP Return Name
0x400003ffffff1730 0x0003482c spinlock+0x2c
0x400003ffffff1730 0x000cad94 issig+0x8c
0x400003ffffff15c0 0x0012befc _sleep+0x24c
0x400003ffffff1490 0x0011d09c read_sleep+0x1ac
0x400003ffffff1390 0x00138660 streams_getmsg+0x1a0
0x400003ffffff1180 0x00139900 soaccept+0x9d8
0x400003ffffff0f20 0x00136050 sodequeue+0xb8
0x400003ffffff0e80 0x0013a1ec accept+0x22c
0x400003ffffff0dc0 0x001574a4 syscall+0x28c
0x400003ffffff0c70 0x00035944 syscallinit+0x54c


Stack Traces for other processors 
=================================


Processor #1
==============  EVENT  ============================
= Event #1 is TOC on CPU #1
= p crash_event_t 0x22030
= p rpb_t 0xc4c370
= Using pc from pim.wide.rp_pcoq_head_hi = 0x129044
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x0000000006bf12b0 0x00129044 idle+0xff4
0x0000000006bf1050 0x0012b804 swidle+0x20


			==================
			= Memory Globals =
			==================

Physical Memory     = 524288 pages (2.00 GB)
Free Memory         = 260409 pages (1017.22 MB)
Average Free Memory = 260409 pages (1017.22 MB)
gpgslim             = 1024 pages (4.00 MB)
lotsfree            = 8192 pages (32.00 MB)
desfree             = 1024 pages (4.00 MB)
minfree             = 256 pages (1.00 MB)

			========================
			= Buffer Cache Globals =
			========================

dbc_max_pct           = 50 %
dbc_min_pct           = 5 %
dbc current pct       = 19.8 %
bufpages              = 103905 pages (405.88 MB)
Number of buf headers = 51618

fixed_size_cache = 0 
dbc_parolemem    = 0 
dbc_stealavg     = 0 
dbc_ceiling      = 262144 pages (1.00 GB)
dbc_nbuf         = 13107 
dbc_bufpages     = 26214 pages (102.40 MB)
dbc_vhandcredit  = 0 
orignbuf         = 0 
origbufpages     = 0 pages

			====================
			= Swap Information =
			====================

swapinfo -mt emulation
======================

             Mb      Mb      Mb   PCT  START/      Mb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev        4096       0    4096    0%       0       -    1  LVM vg00/lv2
reserve       -     445    -445
memory     1539     504    1035   33%
total      5635     949    4686   17%       -       0    -


			======================
			= Network Interfaces =
			======================

Name   PPA   Driver      Interface           Mac       States       IP
              Name      Description        Address    Link IP     Address
--------------------------------------------------------------------------------
lan0    0    btlan3  100BT PCI Built-in 0x00306ec341dd UP   UP   192.168.1.1  
lan1    1    btlan5  100BT PCI Add-on   0x00306e4ac9ab UP   UP   10.76.20.11  
lan2    2    btlan5  100BT PCI Add-on   0x00306e4aa9a5 UP   n/c  n/c          

n/c : means "Not Configured", ifconfig has not been done on this interface


If you want more information, you can use : "lanshow -f"

			====================
			= IOVA Usage Check =
			====================


99% of IOVA still available/free.

		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 2


        s
        t
        a       spin reg eiem_hi           eirr             ipsw
evt cpu t type  dpth src cr15_hi  spl      cr23             cr22
--- --- - ----- ---- --- -------- -------- ---------------- ----------------
0   0   E TOC   3    rpb c6000000 SPLSYS   0000000800000000 0864f91f WLNCRQPDI
                     mpi fffffff0 SPLSCHED
1   1   E TOC   0    rpb fffffff0 SPLSCHED 0000000000000000 0804f91f WCRQPDI

Outstanding external interrupts
===============================

    eirr
cpu bit  SPL      Handler SPL Handler
--- ---- -------- ----------- -------
0   28   SPLSYS   SPLSCHED    take_a_trap

SPL/EIEM values:

0xffffffffffffffff = SPLUSER  Default user mode SPL level.
0xfffffff0ffffffff = SPLSCHED Disable scheduling interrupts.
0xffffff00ffffffff = SPLSOFT  Disable software interrupts (sw triggers off).
0xef00080000000000 = SPLIO    Disable IO modules.
0xc700000000000000 = SPLSYS   Enable clock-resync.
0xc600000000000000 = SPLSYS   Disable system events (eg. hardclock).
0x0000000700000000 = SPL7     Disable the world (eg. PSW_I=0).


			========================
			= Processor Clock Info =
			========================

hardclock_late = 0
itick_per_tick = 7500000
lbolt          = 5844141 (0x592cad)

    event mpi                rpb                delta      clk eiem eirr PSW
cpu type  timeinval          interval timer     secs:ticks od  0,4  0,4  I
--- ----- ------------------ ------------------ ---------- --- ---- ---- ---
0   TOC   0x27f944522516     0x27f944215fbf          0:0   0   1 0  0 0  1 
1   TOC   0x27f943ec8233     0x27f943e9b6d3          0:0   0   1 1  0 0  1 

			=================
			= Load Averages =
			=================


avenrun
=======
13.99  13.38  11.96  

real_run
========
26.976143  25.763230  22.930830  

pwrun ("fast" io wait)
======================
1.000000  1.000000  0.991481  

mp_avenrun
==========
cpu0 : 26.305790  24.976143  22.163160  
cpu1 : 1.670353  1.787088  1.759151  

			======================
			= Thread Information =
			======================

32 Threads ran in the last second
70 Threads ran in the last 5 seconds
117 Threads ran in the last 10 seconds
157 Threads ran in the last minute
264 Threads ran in the last hour

statdaemon ran 79 ticks ago


Most Common Wait Channels
=========================
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
selwait                                            48     319623     29        
vx_inactive_thread_sv+0x8                          25     40626      626       
lvmkd_q                                            6      5823       5823      
async_bufhead                                      4      5829716    5829716   
streams_blk_sync                                   2      5839222    5839222   
svc_run(0x40033180)                                2      5827621    5827621   
svc_run(0x400331c0)                                2      5827620    5827620   
*uptr+0                                            2      335113     46        
streams_mp_sync                                    2      97803      97803     


Most Common Sleep Callers
=========================
                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
_lwp_cond_wait_sys()                               77     5821551    1         
vx_inactive_thread()                               50     462922     626       
select()                                           38     319623     29        
ksleep()                                           37     5821690    1         
wait1()                                            33     5827536    12113     
pm_sigwait()                                       15     5821689    63        
poll()                                             11     319623     48        
nanosleep()                                        8      2603       3         
lvmkd_daemon()                                     6      5823       5823      
async_daemon()                                     4      5829716    5829716   
svc_run()                                          4      5827621    5827620   
sosleep()                                          4      5821545    3295      
getmsg_subr()                                      3      409456     97803     
fifo_rdwr()                                        2      5827687    5605      
streams_getmsg()                                   2      422266     3295      
sigsuspend()                                       2      335113     46        


Idle Globals
============

candidate_idle_spu = 1
migration_cycles = 0

Running Threads (TSRUNPROC) and idle Processors
===============================================


                    TICKS      TICKS                    I TICKS
                    SINCE      SINCE                    C SINCE      NREADY
TID     PID   PPID  RUN        IDLE       PRI SPU STATE S MIGR       FR LO AL COMMAND
------- ----- ----- ---------- ---------- --- --- ----- - ---------  -- -- -- -------
11512   9693  9688  422266     422266     154 0   SYS   N 422266     0  51 0  java 
                               0              1   IDLE  N 3284       0  0  0  

Note: 
FR: free to run on any processor (candidate for thread migration).
LO: locked (via processor affinity/mpctl) to this processor).
AL: Alpha semaphores misses (special scheduling when miss a sema).


Threads waiting on cpu (TSRUN) - sorted by cpu/pri/ticks-since-run
==================================================================

TID     PID   PPID  TICKS      PRI SPU BND GRP SYSCALL        COMMAND
------- ----- ----- ---------- --- --- --- --- -------------- -------
82      33    0     422919     138 0   Y   0   n/a            vxfsd          
68      33    0     424919     138 0   Y   0   n/a            vxfsd          
66      33    0     425715     138 0   Y   0   n/a            vxfsd          
60      33    0     426029     138 0   Y   0   n/a            vxfsd          
62      33    0     426919     138 0   Y   0   n/a            vxfsd          
58      33    0     428911     138 0   Y   0   n/a            vxfsd          
56      33    0     430919     138 0   Y   0   n/a            vxfsd          
84      33    0     432919     138 0   Y   0   n/a            vxfsd          
52      33    0     434919     138 0   Y   0   n/a            vxfsd          
48      33    0     436919     138 0   Y   0   n/a            vxfsd          
46      33    0     438919     138 0   Y   0   n/a            vxfsd          
42      33    0     440919     138 0   Y   0   n/a            vxfsd          
88      33    0     442919     138 0   Y   0   n/a            vxfsd          
44      33    0     443829     138 0   Y   0   n/a            vxfsd          
76      33    0     446892     138 0   Y   0   n/a            vxfsd          
54      33    0     448927     138 0   Y   0   n/a            vxfsd          
72      33    0     450930     138 0   Y   0   n/a            vxfsd          
86      33    0     452930     138 0   Y   0   n/a            vxfsd          
50      33    0     454922     138 0   Y   0   n/a            vxfsd          
80      33    0     456048     138 0   Y   0   n/a            vxfsd          
78      33    0     456930     138 0   Y   0   n/a            vxfsd          
74      33    0     457361     138 0   Y   0   n/a            vxfsd          
64      33    0     458920     138 0   Y   0   n/a            vxfsd          
90      33    0     460930     138 0   Y   0   n/a            vxfsd          
70      33    0     462922     138 0   Y   0   n/a            vxfsd          
484     427   1     419664     154 0   N   0   select         syslogd        
1217    1160  1     5827442    156 0   N   0   read           getty          
12878   10611 10610 86608      158 0   N   0   open           ioscan         
1355    1298  1038  421992     158 0   N   0   open           psmctd         
1354    1297  1038  422215     158 0   N   0   open           memlogd        
12911   10637 10636 12113      178 0   N   0   open           perf.sh        
12893   10626 10625 42101      178 0   N   0   open           perf.sh        
12882   10615 10614 72166      178 0   N   0   open           perf.sh        
12869   10602 10601 102106     178 0   N   0   open           perf.sh        
12861   10597 10596 114809     178 0   N   0   open           login          
12854   10590 10589 132097     178 0   N   0   open           perf.sh        
12844   10580 10579 162177     178 0   N   0   open           perf.sh        
12832   10568 10567 192111     178 0   N   0   open           perf.sh        
12819   10556 10555 222124     178 0   N   0   open           perf.sh        
12809   10546 10545 252182     178 0   N   0   open           perf.sh        
12793   10531 10530 282163     178 0   N   0   open           perf.sh        
1691    1576  1     294004     178 0   N   0   open           dm_core_hw     
12789   10527 10526 303816     178 0   N   0   open           login          
12778   10516 10515 312177     178 0   N   0   open           perf.sh        
12609   10481 10480 342105     178 0   N   0   open           perf.sh        
12381   10370 10369 372061     178 0   N   0   open           perf.sh        
1772    1655  1     378995     178 0   N   0   open           dm_ses_enclosu 
1494    1383  1     383395     178 0   N   0   open           disk_em        
1884    1763  1     383817     178 0   Y   0   sched*         lpmc_em        
12102   10218 10217 402087     178 0   N   0   open           perf.sh        
12042   10201 10200 409727     178 0   N   0   open           login          

```

