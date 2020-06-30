# System panic with data page fault in ATQ_F_dequeue

## Data page fault panic
data page fault in ATQ_F_dequeue routine

```

System Crash Dump Analysis Report
=================================

Symptom
-------
 System panic with data page fault in ATQ_F_dequeue routine
 while system went to memory presure.
 
Root Cause
----------
 It seems there is a bug that results in a data page fault
 panic when memory allocation has had to be delayed.

Action Plan
-----------
Catalog     Text      (16 patches 69MB)
==========  ===============================================
PHCO_21187  cumulative SAM/ObAM patch
PHCO_23651  fsck_vxfs(1M) cumulative patch
PHCO_23791  LVM commands cumulative patch
PHKL_18543  PM/VM/UFS/async/scsi/io/DMAPI/JFS/perf patch
PHKL_20016  2nd CPU not recognized in G70/H70/I70
PHKL_22589  LOFS, select(), IDS/9000 and umount race fix
PHKL_23409  NFS, Large Data Space, kernel memory leak
PHKL_23842  signal,threads,spinlock,scheduler,IDS,q3p
PHKL_27980  VxFS 3.1 cumulative patch: CR_EIEM
PHKL_28150  LVM Cumulative Patch w/Performance Upgrades
PHKL_28766  Probe,IDDS,PM,VM,PA-8700,AIO,T600,FS,PDC,CLK
PHKL_29385  IDS/9000; syscalls; eventports; dup2() race
PHKL_29434  POSIX AIO;getdirentries;MVFS;rcp;mmap/IDS;
PHNE_27902  Cumulative STREAMS Patch
PHNE_28754  ATM PCI, HSC and HP-PB cumulative patch
PHNE_29473  cumulative ARPA Transport patch



Delay Analysis
--------------

			=======================
			= General Information =
			=======================

Dump time Thu Apr  1 05:30:29 2004 UTC-8
System has been up 77 days, 3 hours, 16 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/N4000-36
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : PCXW (360 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 56.99  50.96  38.49  

			================
			= Crash Events =
			================

Panic string : Data page fault
Getting trap information from trap marker at 0xf2e7400.0x400003ffffff0c20

Trap information
================

Type 15: Data TLB Miss Fault/Data Page Fault

Interruption Instruction Register:
	IIR = 0x53970000

Interruption Space and Offset Registers:
	ISR.IOR = 0x86400.0x10104937238

Interruption Instruction Address Queue:
	PCSQ.PCOQ = 0x0.0x25dc7c = ATQ_F_dequeue+0x14

Interrupt Instruction at ATQ_F_dequeue+0x14:
	ldd  0(ret0),arg3

Virtual address information:

VA 0x86400.0x10104937238 does not have a translation.
  Hashing details:
  Primary:
    pdirhash=0x10965c0=htbl[19246]
                            vtag=0x00086501 0x00004937
             hpde2_0_t pde_next   pde_space  pde_vpage
             0x10965c0 0x0000000 0x00000001 0x00004b2e


Stack Trace for Crash event 0
=============================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x993e08
= Using pc from pim.wide.rp_rp_hi = 0x449e4c
==============  EVENT  ============================
SR5=0x0f2e7400
                SP         RP Return Name
0x400003ffffff13c0 0x00449e4c panic+0x14
0x400003ffffff1340 0x0045dac0 report_trap_or_int_and_panic+0x80
0x400003ffffff12c0 0x0017ade8 trap+0xdb8
0x400003ffffff10f0 0x00179b54 thandler+0xd20
+-------------  TRAP  ----------------------------
|  Trap type 15 in KERNEL mode at 0x25dc7c (ATQ_F_dequeue+0x14)
|  p struct save_state 0xf2e7400.0x400003ffffff0c20
+-------------  TRAP  ----------------------------
SR5=0x0f2e7400
                SP         RP Return Name
0x400003ffffff0c20 0x0025dc7c ATQ_F_dequeue+0x14
0x400003ffffff0c20 0x00353ef0 ABC_F0_bufcall_handle+0x88
0x400003ffffff0bb0 0x001658f4 csq_protect+0x2ac
0x400003ffffff0ad0 0x006f9fa0 bufcall_rsrv+0x90
0x400003ffffff09f0 0x006f9a10 str_mem_daemon+0x250
0x400003ffffff0940 0x003d8cc0 main+0x7b0
0x400003ffffff0840 0x0017171c $vstart+0x48
0x400003ffffff07f0 0x002bf64c $locore+0x94

Stack Trace for Crash event 0 with all args
===========================================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x993e08
= Using pc from pim.wide.rp_rp_hi = 0x449e4c
==============  EVENT  ============================
SR5=0x0f2e7400
                SP         RP Return Name
0x400003ffffff13c0 0x00449e4c panic+0x14
0x400003ffffff1340 0x0045dac0 report_trap_or_int_and_panic+0x80
         arg0: 0x0000000000000001
         arg1: 0x000000000000000f
         arg2: 0x400003ffffff0c20
0x400003ffffff12c0 0x0017ade8 trap+0xdb8
         ....  --------n/a-------
         arg1: 0x400003ffffff0c20
0x400003ffffff10f0 0x00179b54 thandler+0xd20
+-------------  TRAP  ----------------------------
|  Trap type 15 in KERNEL mode at 0x25dc7c (ATQ_F_dequeue+0x14)
|  p struct save_state 0xf2e7400.0x400003ffffff0c20
+-------------  TRAP  ----------------------------
SR5=0x0f2e7400
                SP         RP Return Name
0x400003ffffff0c20 0x0025dc7c ATQ_F_dequeue+0x14
         arg0: 0x0000000000aaabe8
0x400003ffffff0c20 0x00353ef0 ABC_F0_bufcall_handle+0x88
         arg0: 0x0000000000aaabe0
0x400003ffffff0bb0 0x001658f4 csq_protect+0x2ac
         arg0: 0x0000000000000000
         arg1: 0x0000000000000000
         arg2: 0x0000000000353e68
         arg3: 0x0000000000aaabe0
         arg4: 0x400003ffffff0a38
         arg5: 0x0000000000000001
0x400003ffffff0ad0 0x006f9fa0 bufcall_rsrv+0x90
0x400003ffffff09f0 0x006f9a10 str_mem_daemon+0x250
0x400003ffffff0940 0x003d8cc0 main+0x7b0
0x400003ffffff0840 0x0017171c $vstart+0x48
0x400003ffffff07f0 0x002bf64c $locore+0x94

			==================
			= Memory Globals =
			==================

Physical Memory     = 131072 pages (512.00 MB)
Free Memory         = 0 pages
Average Free Memory = 0 pages
gpgslim             = 1024 pages (4.00 MB)
lotsfree            = 6706 pages (26.20 MB)
desfree             = 1024 pages (4.00 MB)
minfree             = 256 pages (1.00 MB)

Note:  Free memory is desperately low (freemem less than minfree) !
Note:  We have memory sleepers !
Note:  There are 37 deactivated processes !

deactload: 35.83  25.71  18.76  
maxpendpageouts = 22
pageoutrate     = 22    curr_pgrate = 494
min_pgrate      = 25    max_pgrate  = 25
lowmemdeact     = 20    thrashdeact = 60


			=======================
			= Kernel Memory Usage =
			=======================


----------------------------------------------------------------------
Pfdat processing:

Scanning 121711 pfdat entries (be patient) ...

Warning: Found a P_BAD page (pfn 0x118a8, pfd_t 0x2971300)
Warning: P_BAD is set by the memory diagnostics (dmem scrubbing).
Warning: Found a P_BAD page (pfn 0x126a9, pfd_t 0x29c9360)
Warning: Found a P_BAD page (pfn 0x1f01e, pfd_t 0x2eb5f40)

----------------------------------------------------------------------
Physical memory usage summary (in page/byte/percent):

Physmem             =   131072  512.0m 100%  Physical memory
  Freemem           =        0    0.0k   0%  Free physical memory
  Used              =   131072  512.0m 100%  Used physical memory
    System          =    77204  301.6m  59%  By kernel:
      text          =     2185    8.5m   2%   text
      data          =      365    1.4m   0%   data
      bss           =      936    3.7m   1%   bss
      Static        =    12818   50.1m  10%   for text/static data
      Dynamic       =    41076  160.5m  31%   for dynamic data
      Bufcache      =    23037   90.0m  18%   for buffer cache
      Eqmem         =       17   68.0k   0%   for equiv. mapped memory
      SCmem         =      256    1.0m   0%   for critical memory
    User            =    53815  210.2m  41%  By user processes:
      Uarea         =     3248   12.7m   2%   for thread uareas
    Disowned        =       47  188.0k   0%  Disowned pages
    Bad             =        3   12.0k   0%  Bad

----------------------------------------------------------------------
Kernel dynamic memory usage (in page/byte/percent):

Dynamic             =    41076  160.5m  31%  Kernel dynamic memory
  MALLOC            =    35701  139.5m  27%  Memory buckets
    bucket[5]       =     9515   37.2m   7%   size    32 bytes
    bucket[6]       =       94  376.0k   0%   size    64 bytes
    bucket[7]       =     1225    4.8m   1%   size   128 bytes
    bucket[8]       =     1415    5.5m   1%   size   256 bytes
    bucket[9]       =     1709    6.7m   1%   size   512 bytes
    bucket[10]      =     1127    4.4m   1%   size  1024 bytes
    bucket[11]      =     5506   21.5m   4%   size  2048 bytes
    bucket[12]      =    12289   48.0m   9%   size  4096 bytes
    bucket[13]      =      208  832.0k   0%   size     2 pages
    bucket[14]      =       24   96.0k   0%   size     3 pages
    bucket[15]      =       56  224.0k   0%   size     4 pages
    bucket[16]      =       20   80.0k   0%   size     5 pages
    bucket[17]      =       66  264.0k   0%   size     6 pages
    bucket[18]      =        7   28.0k   0%   size     7 pages
    bucket[19]      =       40  160.0k   0%   size     8 pages
    bucket[20]      =     2400    9.4m   2%   size >   8 pages
  Reserved          =       12   48.0k   0%  Reserved pools
  Kalloc            =     5329   20.8m   4%  kalloc()
    SuperPagePool   =        0    0.0k   0%    Kernel superpage cache
    BufcacheBufs    =     4015   15.7m   3%    Buffer cache bufs
    BufcacheHash    =      320    1.2m   0%    Buffer cache hash heads
    Other           =      994    3.9m   1%    Other...
  Eqalloc           =       34  136.0k   0%  eqalloc()

			=====================
			= User Memory Usage =
			=====================


Top 10 Processes sorted by physical size (in pages):

pid   command        virtual  physical
----- -------------- -------- --------
13282 notificationSe 8702     5108    
19772 NotifyLog      152165   4880    
19773 NotifyLog      152173   4596    
5408  fxaCompress    137921   4198    
18426 rpc.ldmd       136372   3938    
19782 pqact          136293   3674    
20417 NpqFwd         138524   3669    
18437 pqexpire       136179   3657    
2388  proAgent       2679     890     
2030  midaemon       3125     888     

			========================
			= Buffer Cache Globals =
			========================

dbc_max_pct           = 30 %
dbc_min_pct           = 5 %
dbc current pct       = 17.6 %
bufpages              = 23037 pages (89.99 MB)
Number of buf headers = 28161

fixed_size_cache = 0 
dbc_parolemem    = 0 
dbc_stealavg     = 2289 
dbc_ceiling      = 39321 pages (153.60 MB)
dbc_nbuf         = 3276 
dbc_bufpages     = 6553 pages (25.60 MB)
dbc_vhandcredit  = 73269499 
orignbuf         = 0 
origbufpages     = 0 pages

			====================
			= Swap Information =
			====================

swapinfo -mt emulation
======================

             Mb      Mb      Mb   PCT  START/      Mb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev        1024     261     763   25%       0       -    1  LVM vg00/lv2
reserve       -     249    -249
memory      362     231     131   64%
total      1386     741     645   53%       -       0    -


			========================
			= Processor Clock Info =
			========================

hardclock_late = 0
itick_per_tick = 3600000
lbolt          = 666465743 (0x27b975cf)

    event mpi                rpb                delta      clk eiem eirr PSW
cpu type  timeinval          interval timer     secs:ticks od  0,4  0,4  I
--- ----- ------------------ ------------------ ---------- --- ---- ---- ---
0   PANIC 0x88630cb6438e0    0x8862d64383a75        40:59  0   1 0  0 0  1 
1   TOC   0x88630cb646d11    0x88630cb4f5320         0:0   0   1 1  0 0  1 


			=================
			= Syswait Array =
			=================

cpu iowait
--- ------
0   23
1   11


			=================
			= Load Averages =
			=================


avenrun
=======
56.99  50.96  38.49  

real_run
========
1.395185  1.030932  0.830037  

pwrun ("fast" io wait)
======================
100.243308  94.528341  73.443166  

mp_avenrun
==========
cpu0 : 75.019541  67.054208  49.600715  
cpu1 : 38.969246  34.864238  27.387605  

			======================
			= Thread Information =
			======================

25 Threads ran in the last second
29 Threads ran in the last 5 seconds
30 Threads ran in the last 10 seconds
58 Threads ran in the last minute
296 Threads ran in the last hour

statdaemon ran 87 ticks ago


Most Common Wait Channels
=========================
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
selwait                                            37     666442388  119       
vx_rwsleep_lock(0x100c99fa1)                       20     98393      65362     
async_bufhead                                      16     505479013  505479013 
vx_inactive_thread_sv                              16     31835      1835      
memory_sleepers                                    14     66201      32227     
vx_rwsleep_lock(0x104e8df20)                       13     292544     292512    
freemem                                            12     60200      6000      
vx_inactive_thread_sv+0x8                          11     24435      4435      
LVM vg01/lv1                                       9      320445     77098     
ogetblk(0x1007f46d8)                               9      242897     122883    
proc[104]                                          8      666423235  666422190 
vx_svar_sleep_unlock(0x102222c78)                  8      294081     4057      
ogetblk(0x1004d8248)                               5      272863     152799    
lvmkd_q                                            5      1887       1887      
streams_blk_sync                                   2      666457356  666457356 
*uptr+0                                            2      205938694  122608    
locklist+0x13e8                                    2      305846     305164    
streams_mp_sync                                    2      159        59        


Most Common Sleep Callers
=========================
                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
ksleep()                                           75     666423444  8         
vx_rwsleep_lock()                                  35     310228     65362     
wait1()                                            34     666423235  122813    
vx_inactive_thread()                               29     31835      1835      
select()                                           28     666442388  119       
ogetblk()                                          27     320445     122733    
sysmemreserve()                                    26     66201      6000      
hpstreams_read_int()                               24     666423219  666421952 
vx_event_wait()                                    23     4060       1         
async_daemon()                                     16     505479013  505479013 
sosleep()                                          14     666421499  58504     
biowait()                                          12     320594     74113     
poll()                                             11     666438672  1033      
fifo_rdwr()                                        8      302836     98394     
vx_svar_sleep_unlock()                             8      294081     4057      
lvmkd_daemon()                                     5      1887       1887      
getmsg_subr()                                      3      666446198  666421778 
streams_getmsg()                                   3      666423197  666422158 
pm_sigwait()                                       3      15084      66        
locked()                                           2      305846     305164    

```

