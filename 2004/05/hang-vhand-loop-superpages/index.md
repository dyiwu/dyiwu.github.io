# Vhand loops in unlock superpages lead system hang.

## Vhand run in a loop to unlock a superpage and monopolizes the cpu for a while.

```

System crash dump analysis report
=================================

Symptom
-------
  System behaved as hang, no response on user's commands,
  TOC dump be taken for root cause finding.

Root Cause
----------
  The main problem is lack of free memory.  
  This causes vhand to run and in a loop to unlock a superpage and
  monopolizes the cpu for a while.

  The system wasn't actually hung, but it was running so slowly that it
  appeared to be hung.

Action Plan
-----------
  Please install PHKL_28695
    Cumulative VM, Psets, Preemption, PRM, MRG

Detail Analysis
---------------
			=======================
			= General Information =
			=======================

Dump time Fri May 21 15:01:05 2004 UTC-8
System has been up 36 days, 20 hours, 29 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/SD32000
HP-UX version    : B.11.11 (64-bit Kernel)
Number of CPU's  : 15
Disabled CPU's   : 8
CPU type         : PCXW+ (875 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.21  0.22  0.29  

			================
			= Crash Events =
			================
Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is TOC on CPU #3
= p crash_event_t 0x4acb000
= p rpb_t 0x5a9f130
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198698
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cda47f0 0x04198698 preArbitration+0x1a8
0x000000006cda46f0 0x04197ee8 wait_for_lock+0xf8
0x000000006cda45b0 0x04014bfc sl_retry+0x1c
0x000000006cda4530 0x046dc888 pset_get_num_spu+0x18
0x000000006cda44a0 0x046da73c pset_idle_loop+0x70c
0x000000006cda42c0 0x04159330 idle+0xb88
0x000000006cda4050 0x04156d6c swidle+0x28


Stack Traces for other processors 
=================================

Processor #0
==============  EVENT  ============================
= Event #4 is TOC on CPU #0
= p crash_event_t 0x4acb0c0
= p rpb_t 0x4ac4eb0
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198680
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cd9b7f0 0x04198680 preArbitration+0x190
0x000000006cd9b6f0 0x04197ee8 wait_for_lock+0xf8
0x000000006cd9b5b0 0x04014bfc sl_retry+0x1c
0x000000006cd9b530 0x046dc888 pset_get_num_spu+0x18
0x000000006cd9b4a0 0x046da73c pset_idle_loop+0x70c
0x000000006cd9b2c0 0x04159330 idle+0xb88
0x000000006cd9b050 0x04156d6c swidle+0x28

Processor #1
==============  EVENT  ============================
= Event #5 is TOC on CPU #1
= p crash_event_t 0x4acb0f0
= p rpb_t 0x5a9e370
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198698
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cd9e7f0 0x04198698 preArbitration+0x1a8
0x000000006cd9e6f0 0x04197ee8 wait_for_lock+0xf8
0x000000006cd9e5b0 0x04014bfc sl_retry+0x1c
0x000000006cd9e530 0x046dc888 pset_get_num_spu+0x18
0x000000006cd9e4a0 0x046da73c pset_idle_loop+0x70c
0x000000006cd9e2c0 0x04159330 idle+0xb88
0x000000006cd9e050 0x04156d6c swidle+0x28

Processor #2
==============  EVENT  ============================
= Event #6 is TOC on CPU #2
= p crash_event_t 0x4acb120
= p rpb_t 0x5a9ea50
= Using pc from pim.wide.rp_pcoq_head_hi = 0x46da2f8
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cda14a0 0x046da2f8 pset_idle_loop+0x2c8
0x000000006cda12c0 0x04159330 idle+0xb88
0x000000006cda1050 0x04156d6c swidle+0x28

Processor #4
==============  EVENT  ============================
= Event #1 is TOC on CPU #4
= p crash_event_t 0x4acb030
= p rpb_t 0x5a9f810
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198698
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cda77f0 0x04198698 preArbitration+0x1a8
0x000000006cda76f0 0x04197ee8 wait_for_lock+0xf8
0x000000006cda75b0 0x04014bfc sl_retry+0x1c
0x000000006cda7530 0x046dc888 pset_get_num_spu+0x18
0x000000006cda74a0 0x046da73c pset_idle_loop+0x70c
0x000000006cda72c0 0x04159330 idle+0xb88
0x000000006cda7050 0x04156d6c swidle+0x28

Processor #5
==============  EVENT  ============================
= Event #2 is TOC on CPU #5
= p crash_event_t 0x4acb060
= p rpb_t 0x5a9fef0
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198698
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cdaa810 0x04198698 preArbitration+0x1a8
0x000000006cdaa710 0x04197ee8 wait_for_lock+0xf8
0x000000006cdaa5d0 0x04014bfc sl_retry+0x1c
0x000000006cdaa550 0x046dc6b0 pset_get_nextspu+0x40
0x000000006cdaa4a0 0x046da83c pset_idle_loop+0x80c
0x000000006cdaa2c0 0x04159330 idle+0xb88
0x000000006cdaa050 0x04156d6c swidle+0x28

Processor #6
==============  EVENT  ============================
= Event #3 is TOC on CPU #6
= p crash_event_t 0x4acb090
= p rpb_t 0x5aa05d0
= Using pc from pim.wide.rp_pcoq_head_hi = 0x401502c
==============  EVENT  ============================
SR5=0x044e5800
                SP         RP Return Name
0x400003ffffff1658 0x0401502c mpn_b_vsema+0x30
0x400003ffffff1658 0x04152050 superpage_unlock+0x90
0x400003ffffff15b8 0x04166b30 devswap_vfdcheck+0x300
0x400003ffffff14a8 0x044af49c for_val3+0x5c
0x400003ffffff1408 0x0415e674 for_val2+0x29c
0x400003ffffff12e8 0x0415e514 for_val2+0x13c
0x400003ffffff11c8 0x0415e544 for_val2+0x16c
0x400003ffffff10a8 0x0415e544 for_val2+0x16c
0x400003ffffff0f88 0x0415e3c4 foreach_valid+0x54
0x400003ffffff0ef8 0x04105be4 devswap_pageout+0x104
0x400003ffffff0dc8 0x041625d4 stealpages+0x6c
0x400003ffffff0d18 0x04141548 vhand_core+0x1c8
0x400003ffffff0c48 0x04179410 vhand_global_pager+0x3d8
0x400003ffffff0b68 0x041789d0 vhand+0x378
0x400003ffffff09c8 0x0439c0bc im_vhand+0xd4
0x400003ffffff0938 0x044af254 DoCalllist+0x3c
0x400003ffffff08b8 0x0439c4b0 main+0x28
0x400003ffffff0848 0x041557cc $vstart+0x48
0x400003ffffff07f8 0x041db2e4 $locore+0x94

			==================
			= Memory Globals =
			==================

Physical Memory     = 3922944 pages (14.96 GB)
Free Memory         = 6203 pages (24.23 MB)
Average Free Memory = 6174 pages (24.12 MB)
desfree             = 13312 pages (52.00 MB)
minfree             = 6400 pages (25.00 MB)

Note:  Free memory is desperately low (freemem < minfree) !
Note:  We have memory sleepers !


			=======================
			= Kernel Memory Usage =
			=======================


----------------------------------------------------------------------
Pfdat processing:

Scanning 3829415 pfdat entries (be patient) ...

----------------------------------------------------------------------
Physical memory usage summary (in page/byte/percent):

Physmem             =  3922944   15.0g 100%  Physical memory
  Freemem           =     6203   24.2m   0%  Free physical memory
  Used              =  3916741   14.9g 100%  Used physical memory
    System          =   927308    3.5g  24%  By kernel:
      Static        =   183463  716.7m   5%   for text/static data
      Dynamic       =   343747    1.3g   9%   for dynamic data
      Bufcache      =   392294    1.5g  10%   for buffer cache
      Eqmem         =       74  296.0k   0%   for equiv. mapped memory
      SCmem         =     7730   30.2m   0%   for critical memory
    User            =  2989425   11.4g  76%  By user processes:
      Uarea         =    12792   50.0m   0%   for thread uareas
    Disowned        =        8   32.0k   0%  Disowned pages

----------------------------------------------------------------------
Kernel dynamic memory usage (in page/byte/percent):

Dynamic             =   343747    1.3g   9%  Kernel dynamic memory
  Arenas            =   294108    1.1g   7%  Kernel arenas
    M_TEMP          =   202968  792.8m   5%  
    M_SPINLOCK      =    15856   61.9m   0%  
    M_LVM           =    12959   50.6m   0%  
    VFD_BT_NODE     =    10341   40.4m   0%  
    KMEM_ALLOC      =     9999   39.1m   0%  
    M_SWAP          =     7791   30.4m   0%  
    ALLOCB_MBLK_LM  =     6594   25.8m   0%  
    M_REG           =     2724   10.6m   0%  
    M_PREG          =     2666   10.4m   0%  
    M_STRQUEUE      =     2108    8.2m   0%  
    M_IOSYS         =     1824    7.1m   0%  
    KMEM_VARFLIST_H =     1776    6.9m   0%  
    M_KTHREAD       =     1605    6.3m   0%  
    FD_CHUNK_T_AREN =     1401    5.5m   0%  
    M_VXVM          =     1367    5.3m   0%  
    Other           =    12129   47.4m   0%  Other arenas...
  Kalloc            =    49484  193.3m   1%  kalloc()
    SuperPagePool   =       58  232.0k   0%    Kernel superpage cache
    BufcacheBufs    =    35411  138.3m   1%    Buffer cache bufs
    BufcacheHash    =    10240   40.0m   0%    Buffer cache hash heads
    Other           =     3775   14.7m   0%    Other...
  Eqalloc           =      155  620.0k   0%  eqalloc()

			=====================
			= User Memory Usage =
			=====================

Top 10 Processes sorted by physical size (in pages):

pid   command        virtual  physical
----- -------------- -------- --------
3682  java           67944    33877   
14696 oracle         2077939  25009   
27920 dbsnmp         18397    13238   
26303 oracle         2067683  12929   
3610  oracle         2061105  12299   
15268 oracle         2061539  11921   
15847 oracle         2061361  11882   
3594  oracle         2060593  11803   
3592  oracle         2060593  11803   
2954  vxsvc          13477    9457    

			========================
			= Buffer Cache Globals =
			========================

dbc_max_pct           = 10 %
dbc_min_pct           = 5 %
dbc current pct       = 10.0 %
bufpages              = 392294 pages (1.50 GB)
Number of buf headers = 241740

fixed_size_cache = 0 
dbc_parolemem    = 0 
dbc_stealavg     = 0 
dbc_ceiling      = 392294 pages (1.50 GB)
dbc_nbuf         = 98073 
dbc_bufpages     = 196147 pages (766.20 MB)
dbc_vhandcredit  = 142419 
orignbuf         = 0 
origbufpages     = 0 pages
================================
libp4 (8.27): Opening ./vmunix ./INDEX

gunzip ./vmunix.gz ...
Loading symbols from ./vmunix
Kernel TEXT pages not requested in crashconf
Will use an artificial mapping from a.out TEXT pages
Loading symbols from ./SEOS
Warning: Load vt failed on this module
Warning: Load debug_header failed on this module

		      crashinfo (3.5) output

			=====================
			= Table Of Contents =
			=====================


* General Information
* Crash Events
* Message Buffer
* Memory Globals
* Kernel Memory Usage
* User Memory Usage
* Buffer Cache Globals
* Swap Information
* Global Error Counters / kmem_writes
* Network Interfaces
* Crash Event / Processor Information
* Processor Clock Info
* Load Averages
* Beta Semaphore Checks
* Thread Information
* Kernel Patches

			=======================
			= General Information =
			=======================


Note: System appears to be using Virtual Partitions (vpars) !

Dump time Fri May 21 15:01:05 2004 UTC-8
System has been up 36 days, 20 hours, 29 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/SD32000
HP-UX version    : B.11.11 (64-bit Kernel)
Number of CPU's  : 15
Disabled CPU's   : 8
CPU type         : PCXW+ (875 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.21  0.22  0.29  

			================
			= Crash Events =
			================


Note: We have 8 disabled cpus !
As this is a vpars system you should not get a crash event
for the disabled cpus.


Note: CPU 7 is disabled


Note: CPU 8 is disabled


Note: CPU 9 is disabled


Note: CPU 10 is disabled


Note: CPU 11 is disabled


Note: CPU 12 is disabled


Note: CPU 13 is disabled


Note: CPU 14 is disabled


Note: Crash event 0 was a TOC !


Note: This seems to be a user initiated TOC !
To get a better understanding why the system was brought down,
please go to: 
"http://wtec.cup.hp.com/~hpux/crash/FirstPassWeb/PA/contents.htm#TOC"

Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is TOC on CPU #3
= p crash_event_t 0x4acb000
= p rpb_t 0x5a9f130
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198698
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cda47f0 0x04198698 preArbitration+0x1a8
0x000000006cda46f0 0x04197ee8 wait_for_lock+0xf8
0x000000006cda45b0 0x04014bfc sl_retry+0x1c
0x000000006cda4530 0x046dc888 pset_get_num_spu+0x18
0x000000006cda44a0 0x046da73c pset_idle_loop+0x70c
0x000000006cda42c0 0x04159330 idle+0xb88
0x000000006cda4050 0x04156d6c swidle+0x28


Stack Traces for other processors 
=================================


Processor #0
==============  EVENT  ============================
= Event #4 is TOC on CPU #0
= p crash_event_t 0x4acb0c0
= p rpb_t 0x4ac4eb0
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198680
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cd9b7f0 0x04198680 preArbitration+0x190
0x000000006cd9b6f0 0x04197ee8 wait_for_lock+0xf8
0x000000006cd9b5b0 0x04014bfc sl_retry+0x1c
0x000000006cd9b530 0x046dc888 pset_get_num_spu+0x18
0x000000006cd9b4a0 0x046da73c pset_idle_loop+0x70c
0x000000006cd9b2c0 0x04159330 idle+0xb88
0x000000006cd9b050 0x04156d6c swidle+0x28

Processor #1
==============  EVENT  ============================
= Event #5 is TOC on CPU #1
= p crash_event_t 0x4acb0f0
= p rpb_t 0x5a9e370
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198698
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cd9e7f0 0x04198698 preArbitration+0x1a8
0x000000006cd9e6f0 0x04197ee8 wait_for_lock+0xf8
0x000000006cd9e5b0 0x04014bfc sl_retry+0x1c
0x000000006cd9e530 0x046dc888 pset_get_num_spu+0x18
0x000000006cd9e4a0 0x046da73c pset_idle_loop+0x70c
0x000000006cd9e2c0 0x04159330 idle+0xb88
0x000000006cd9e050 0x04156d6c swidle+0x28

Processor #2
==============  EVENT  ============================
= Event #6 is TOC on CPU #2
= p crash_event_t 0x4acb120
= p rpb_t 0x5a9ea50
= Using pc from pim.wide.rp_pcoq_head_hi = 0x46da2f8
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cda14a0 0x046da2f8 pset_idle_loop+0x2c8
0x000000006cda12c0 0x04159330 idle+0xb88
0x000000006cda1050 0x04156d6c swidle+0x28

Processor #4
==============  EVENT  ============================
= Event #1 is TOC on CPU #4
= p crash_event_t 0x4acb030
= p rpb_t 0x5a9f810
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198698
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cda77f0 0x04198698 preArbitration+0x1a8
0x000000006cda76f0 0x04197ee8 wait_for_lock+0xf8
0x000000006cda75b0 0x04014bfc sl_retry+0x1c
0x000000006cda7530 0x046dc888 pset_get_num_spu+0x18
0x000000006cda74a0 0x046da73c pset_idle_loop+0x70c
0x000000006cda72c0 0x04159330 idle+0xb88
0x000000006cda7050 0x04156d6c swidle+0x28

Processor #5
==============  EVENT  ============================
= Event #2 is TOC on CPU #5
= p crash_event_t 0x4acb060
= p rpb_t 0x5a9fef0
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4198698
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x000000006cdaa810 0x04198698 preArbitration+0x1a8
0x000000006cdaa710 0x04197ee8 wait_for_lock+0xf8
0x000000006cdaa5d0 0x04014bfc sl_retry+0x1c
0x000000006cdaa550 0x046dc6b0 pset_get_nextspu+0x40
0x000000006cdaa4a0 0x046da83c pset_idle_loop+0x80c
0x000000006cdaa2c0 0x04159330 idle+0xb88
0x000000006cdaa050 0x04156d6c swidle+0x28

Processor #6
==============  EVENT  ============================
= Event #3 is TOC on CPU #6
= p crash_event_t 0x4acb090
= p rpb_t 0x5aa05d0
= Using pc from pim.wide.rp_pcoq_head_hi = 0x401502c
==============  EVENT  ============================
SR5=0x044e5800
                SP         RP Return Name
0x400003ffffff1658 0x0401502c mpn_b_vsema+0x30
0x400003ffffff1658 0x04152050 superpage_unlock+0x90
0x400003ffffff15b8 0x04166b30 devswap_vfdcheck+0x300
0x400003ffffff14a8 0x044af49c for_val3+0x5c
0x400003ffffff1408 0x0415e674 for_val2+0x29c
0x400003ffffff12e8 0x0415e514 for_val2+0x13c
0x400003ffffff11c8 0x0415e544 for_val2+0x16c
0x400003ffffff10a8 0x0415e544 for_val2+0x16c
0x400003ffffff0f88 0x0415e3c4 foreach_valid+0x54
0x400003ffffff0ef8 0x04105be4 devswap_pageout+0x104
0x400003ffffff0dc8 0x041625d4 stealpages+0x6c
0x400003ffffff0d18 0x04141548 vhand_core+0x1c8
0x400003ffffff0c48 0x04179410 vhand_global_pager+0x3d8
0x400003ffffff0b68 0x041789d0 vhand+0x378
0x400003ffffff09c8 0x0439c0bc im_vhand+0xd4
0x400003ffffff0938 0x044af254 DoCalllist+0x3c
0x400003ffffff08b8 0x0439c4b0 main+0x28
0x400003ffffff0848 0x041557cc $vstart+0x48
0x400003ffffff07f8 0x041db2e4 $locore+0x94
Processor number 7 disabled!

Note: CPU 7 is disabled


Processor #7
Can't find the mpinfo[0].prochpa into the crash event table
Processor number 8 disabled!

Note: CPU 8 is disabled


Processor #8
Can't find the mpinfo[0].prochpa into the crash event table
Processor number 9 disabled!

Note: CPU 9 is disabled


Processor #9
Can't find the mpinfo[0].prochpa into the crash event table
Processor number 10 disabled!

Note: CPU 10 is disabled


Processor #10
Can't find the mpinfo[0].prochpa into the crash event table
Processor number 11 disabled!

Note: CPU 11 is disabled


Processor #11
Can't find the mpinfo[0].prochpa into the crash event table
Processor number 12 disabled!

Note: CPU 12 is disabled


Processor #12
Can't find the mpinfo[0].prochpa into the crash event table
Processor number 13 disabled!

Note: CPU 13 is disabled


Processor #13
Can't find the mpinfo[0].prochpa into the crash event table
Processor number 14 disabled!

Note: CPU 14 is disabled


Processor #14
Can't find the mpinfo[0].prochpa into the crash event table

			==================
			= Message Buffer =
			==================

excessive
   errors from the I/O subsystem.  8 I/O error entries were lost.
DIAGNOSTIC SYSTEM WARNING:
   The diagnostic logging facility has started receiving excessive
   errors from the I/O subsystem.  I/O error entries will be lost
   until the cause of the excessive I/O logging is corrected.
   If the diaglogd daemon is not active, use the Daemon Startup command
   in stm to start it.
   If the diaglogd daemon is active, use the logtool utility in stm
   to determine which I/O subsystem is logging excessive errors.
LVM: Performed a switch for Lun ID = 0 (pv = 0x0000000088142080), from raw device 0x1f0d1700 (with priority: 0, and current flags: 0x40) to raw device 0x1f0c1700 (with priority: 1, and current flags: 0x0).
LVM: Performed a switch for Lun ID = 0 (pv = 0x00000000807d2800), from raw device 0x1f0d2500 (with priority: 0, and current flags: 0x40) to raw device 0x1f0c2500 (with priority: 1, and current flags: 0x0).
LVM: Recovered Path (device 0x1f0d2500) to PV 1 in VG 9.
LVM: Performed a switch for Lun ID = 0 (pv = 0x00000000807d2800), from raw device 0x1f0c2500 (with priority: 1, and current flags: 0x0) to raw device 0x1f0d2500 (with priority: 0, and current flags: 0x0).
LVM: Recovered Path (device 0x1f0d2100) to PV 1 in VG 7.
LVM: Recovered Path (device 0x1f0d2300) to PV 1 in VG 8.
LVM: Recovered Path (device 0x1f0d2700) to PV 1 in VG 10.
LVM: Recovered Path (device 0x1f0d1700) to PV 1 in VG 6.
LVM: Performed a switch for Lun ID = 0 (pv = 0x0000000088142080), from raw device 0x1f0c1700 (with priority: 1, and current flags: 0x0) to raw device 0x1f0d1700 (with priority: 0, and current flags: 0x0).
LVM: Recovered Path (device 0x1f0d3100) to PV 1 in VG 11.
LVM: Performed a switch for Lun ID = 0 (pv = 0x00000000844fa840), from raw device 0x1f0c3100 (with priority: 1, and current flags: 0x0) to raw device 0x1f0d3100 (with priority: 0, and current flags: 0x0).
LVM: Recovered Path (device 0x1f0d0700) to PV 1 in VG 2.
LVM: Recovered Path (device 0x1f0d1100) to PV 1 in VG 3.
LVM: Recovered Path (device 0x1f0d1300) to PV 1 in VG 4.
LVM: Restored PV 1 to VG 6.
LVM: Restored PV 1 to VG 7.
LVM: Restored PV 1 to VG 8.
LVM: Restored PV 1 to VG 9.
LVM: Restored PV 1 to VG 10.
LVM: Restored PV 1 to VG 11.
LVM: Restored PV 1 to VG 2.
LVM: Restored PV 1 to VG 3.
LVM: Restored PV 1 to VG 4.
s: 0x0) to raw device 0x1f0c1000 (with priority: 0, and current flags: 0x0).
LVM: Recovered Path (device 0x1f0c2000) to PV 0 in VG 7.
LVM: Performed a switch for Lun ID = 0 (pv = 0x000000007c6fb080), from raw device 0x1f0d2000 (with priority: 1, and current flags: 0x0) to raw device 0x1f0c2000 (with priority: 0, and current flags: 0x0).
LVM: Restored PV 0 to VG 2.
LVM: Recovered Path (device 0x1f0c1200) to PV 0 in VG 4.
LVM: Recovered Path (device 0x1f0c1400) to PV 0 in VG 5.
LVM: Recovered Path (device 0x1f0c2400) to PV 0 in VG 9.
LVM: Recovered Path (device 0x1f0c2600) to PV 0 in VG 10.
LVM: Recovered Path (device 0x1f0c3000) to PV 0 in VG 11.
LVM: Restored PV 0 to VG 3.
LVM: Restored PV 0 to VG 4.
LVM: Restored PV 0 to VG 5.
LVM: Restored PV 0 to VG 7.
LVM: Restored PV 0 to VG 9.
LVM: Restored PV 0 to VG 10.
LVM: Restored PV 0 to VG 11.
DIAGNOSTIC SYSTEM WARNING:
   The diagnostic logging facility is no longer receiving excessive
   errors from the I/O subsystem.  8 I/O error entries were lost.
LVM: Performed a switch for Lun ID = 0 (pv = 0x00000000844fa840), from raw device 0x1f0d3100 (with priority: 0, and current flags: 0x40) to raw device 0x1f0c3100 (with priority: 1, and current flags: 0x0).
DIAGNOSTIC SYSTEM WARNING:
   The diagnostic logging facility has started receiving excessive
   errors from the I/O subsystem.  I/O error entries will be lost
   until the cause of the excessive I/O logging is corrected.
   If the diaglogd daemon is not active, use the Daemon Startup command
   in stm to start it.
   If the diaglogd daemon is active, use the logtool utility in stm
   to determine which I/O subsystem is logging excessive errors.
DIAGNOSTIC SYSTEM WARNING:
   The diagnostic logging facility is no longer receiving 

			==================
			= Memory Globals =
			==================

Physical Memory     = 3922944 pages (14.96 GB)
Free Memory         = 6203 pages (24.23 MB)
Average Free Memory = 6174 pages (24.12 MB)
desfree             = 13312 pages (52.00 MB)
minfree             = 6400 pages (25.00 MB)

Note:  Free memory is desperately low (freemem < minfree) !


Note:  We have memory sleepers !


			=======================
			= Kernel Memory Usage =
			=======================


----------------------------------------------------------------------
Pfdat processing:

Scanning 3829415 pfdat entries (be patient) ...


----------------------------------------------------------------------
Physical memory usage summary (in page/byte/percent):

Physmem             =  3922944   15.0g 100%  Physical memory
  Freemem           =     6203   24.2m   0%  Free physical memory
  Used              =  3916741   14.9g 100%  Used physical memory
    System          =   927308    3.5g  24%  By kernel:
      Static        =   183463  716.7m   5%   for text/static data
      Dynamic       =   343747    1.3g   9%   for dynamic data
      Bufcache      =   392294    1.5g  10%   for buffer cache
      Eqmem         =       74  296.0k   0%   for equiv. mapped memory
      SCmem         =     7730   30.2m   0%   for critical memory
    User            =  2989425   11.4g  76%  By user processes:
      Uarea         =    12792   50.0m   0%   for thread uareas
    Disowned        =        8   32.0k   0%  Disowned pages

----------------------------------------------------------------------
Kernel dynamic memory usage (in page/byte/percent):

Dynamic             =   343747    1.3g   9%  Kernel dynamic memory
  Arenas            =   294108    1.1g   7%  Kernel arenas
    M_TEMP          =   202968  792.8m   5%  
    M_SPINLOCK      =    15856   61.9m   0%  
    M_LVM           =    12959   50.6m   0%  
    VFD_BT_NODE     =    10341   40.4m   0%  
    KMEM_ALLOC      =     9999   39.1m   0%  
    M_SWAP          =     7791   30.4m   0%  
    ALLOCB_MBLK_LM  =     6594   25.8m   0%  
    M_REG           =     2724   10.6m   0%  
    M_PREG          =     2666   10.4m   0%  
    M_STRQUEUE      =     2108    8.2m   0%  
    M_IOSYS         =     1824    7.1m   0%  
    KMEM_VARFLIST_H =     1776    6.9m   0%  
    M_KTHREAD       =     1605    6.3m   0%  
    FD_CHUNK_T_AREN =     1401    5.5m   0%  
    M_VXVM          =     1367    5.3m   0%  
    Other           =    12129   47.4m   0%  Other arenas...
  Kalloc            =    49484  193.3m   1%  kalloc()
    SuperPagePool   =       58  232.0k   0%    Kernel superpage cache
    BufcacheBufs    =    35411  138.3m   1%    Buffer cache bufs
    BufcacheHash    =    10240   40.0m   0%    Buffer cache hash heads
    Other           =     3775   14.7m   0%    Other...
  Eqalloc           =      155  620.0k   0%  eqalloc()

			=====================
			= User Memory Usage =
			=====================


Top 10 Processes sorted by physical size (in pages):

pid   command        virtual  physical
----- -------------- -------- --------
3682  java           67944    33877   
14696 oracle         2077939  25009   
27920 dbsnmp         18397    13238   
26303 oracle         2067683  12929   
3610  oracle         2061105  12299   
15268 oracle         2061539  11921   
15847 oracle         2061361  11882   
3594  oracle         2060593  11803   
3592  oracle         2060593  11803   
2954  vxsvc          13477    9457    

			========================
			= Buffer Cache Globals =
			========================

dbc_max_pct           = 10 %
dbc_min_pct           = 5 %
dbc current pct       = 10.0 %
bufpages              = 392294 pages (1.50 GB)
Number of buf headers = 241740

fixed_size_cache = 0 
dbc_parolemem    = 0 
dbc_stealavg     = 0 
dbc_ceiling      = 392294 pages (1.50 GB)
dbc_nbuf         = 98073 
dbc_bufpages     = 196147 pages (766.20 MB)
dbc_vhandcredit  = 142419 
orignbuf         = 0 
origbufpages     = 0 pages

			====================
			= Swap Information =
			====================

swapinfo -mt emulation:

             Mb      Mb      Mb   PCT  START/      Mb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev       30720      11   30709    0%       0       -    1  /dev/vg00/lvol2
reserve       -   12440  -12440
memory    11884    2188    9696   18%
total     42604   14639   27965   34%       -       0    -

swapmem_on  = 1 
swapmem_cnt = 2482256 pages (9.47 GB)
swapmem_max = 3042479 pages (11.61 GB)
swapspc_cnt = 4676734 pages (17.84 GB)
swapspc_max = 7864320 pages (30.00 GB)

		=======================================
		= Global Error Counters / kmem_writes =
		=======================================

scsi_bioerrors                    = 26
	scsi_bioerrors_logged             = 0
	scsi_async_write_bioerrors        = 0
	scsi_async_write_bioerrors_logged = 0
	sd_strategy_bioerrors             = 6
	sd_strategy_async_write_bioerrors = 0
	sd_epowerf_bioerrors              = 17
	sd_epowerf_async_write_bioerrors  = 0

Note:  There are scsi_bioerrors ! For more information go to: 
"http://wtec.cup.hp.com/~hpux/crash/info/scsi_bioerrors.htm"


			======================
			= Network Interfaces =
			======================

Name   PPA   Driver      Interface           Mac       States       IP
              Name      Description        Address    Link IP     Address
--------------------------------------------------------------------------------
clic1   1    clic    Hyperfabric        0x000000000000 DOWN n/c  n/c          
lan3    3    btlan   100BT 4 port / 11i 0x00306e4a04cf UP   DOWN 0.0.0.0      
lan4    4    btlan   100BT 4 port / 11i 0x00306e3759b8 UP   UP   192.168.10.3 
lan5    5    igelan  Gigabit            0x00306e49a768 UP   UP   10.6.250.13  
                                                            UP   10.6.250.162 

n/c : means "Not Configured", ifconfig has not been done on this interface


If you want more information, you can use : "lanshow -f"

		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 15


        s
        t
        a       spin reg eiem/spl         eirr             ipsw            
evt cpu t type  dpth src cr15             cr23             cr22            
--- --- - ----- ---- --- ---------------- ---------------- ----------------
0   3   E TOC   1    rpb c400000000000000 0800000100000000 080cff1f WBCRQPDI
                     mpi fffffff0ffffffff
1   4   E TOC   1    rpb c400000000000000 0800000100000000 084cff1f WLBCRQPDI
                     mpi fffffff0ffffffff
2   5   E TOC   1    rpb c400000000000000 0800000100000000 084cfd1f WLBCRQPDI
                     mpi fffffff0ffffffff
3   6   E TOC   0    rpb c400000000000000 0000000900000000 0844f11f WLCRQPDI
4   0   E TOC   1    rpb c400000000000000 0000000000000000 0804f91f WCRQPDI
                     mpi fffffff0ffffffff
5   1   E TOC   1    rpb c400000000000000 0800000100000000 084cff1f WLBCRQPDI
                     mpi fffffff0ffffffff
6   2   E TOC   0    rpb fffffff0ffffffff 0000000100000000 0844c41f WLCRQPDI

Outstanding external interrupts
===============================

    eirr
cpu bit  SPL                Handler SPL        Handler
--- ---- --------           -----------        -------
1   4    SPL6/SPINLOCK_EIEM SPL6/SPINLOCK_EIEM clock_int
1   31   SPL6/SPINLOCK_EIEM SPLNOPREEMPT       take_a_trap
2   31   SPLNOPREEMPT       SPLNOPREEMPT       take_a_trap
3   4    SPL6/SPINLOCK_EIEM SPL6/SPINLOCK_EIEM clock_int
3   31   SPL6/SPINLOCK_EIEM SPLNOPREEMPT       take_a_trap
4   4    SPL6/SPINLOCK_EIEM SPL6/SPINLOCK_EIEM clock_int
4   31   SPL6/SPINLOCK_EIEM SPLNOPREEMPT       take_a_trap
5   4    SPL6/SPINLOCK_EIEM SPL6/SPINLOCK_EIEM clock_int
5   31   SPL6/SPINLOCK_EIEM SPLNOPREEMPT       take_a_trap
6   28   SPL6/SPINLOCK_EIEM SPLNOPREEMPT       take_a_trap
6   31   SPL6/SPINLOCK_EIEM SPLNOPREEMPT       take_a_trap

SPL/EIEM values:

0xfffffffeffffffff = SPLPREEMPTOK - Default user mode SPL level.
0xfffffff0ffffffff = SPLNOPREEMPT - Disable kernel preemption (scheduling interrupt off).
0xffffff00ffffffff = SPL2 - Disable software interrupt (software triggers off).
0xed00080000000000 = SPL5 - Disable IO modules.
0xc500000000000000 = SPL6+CLOCK_RESYNC - Disable hardclock+enable clock-resync.
0xc400000000000000 = SPL6 - Disable hardclock.
0x0000000700000000 = SPL7/PSW_I=0 - Disable the world.


			========================
			= Processor Clock Info =
			========================

hardclock_late = 0
itick_per_tick = 8750000
lbolt          = 318429804 (0x12fada6c)

    mpi                interval                         clk eiem eirr PSW
cpu timeinval          timer              delta (ticks) od  0,4  0,4  I
--- ------------------ ------------------ ------------- --- ---- ---- ---
0   0x1f22b8623c3f78   0x1f22b86222cb7b   0             0   1 0  0 0  1 
1   0x1f22b88278fa9c   0x1f22b88799164b   -9            1   1 0  0 1  1 
2   0x1f22b88c283aa4   0x1f22b88c12becf   0             0   1 1  0 0  1 
3   0x1f22b1a0d674a8   0x1f22b1a3c7a7f3   -5            1   1 0  0 1  1 
4   0x1f22b858161d61   0x1f22b858ecaa7f   -1            1   1 0  0 1  1 
5   0x1f22b856b38184   0x1f22b85aa61f4f   -7            1   1 0  0 1  1 
6   0x1f22b85efa8978   0x1f22b85eba329b   0             0   1 0  0 0  1 

			=================
			= Load Averages =
			=================


avenrun
=======
0.21  0.22  0.29  

mp_avenrun
==========
cpu0 : 0.000000  0.000000  0.000000  
cpu1 : 0.000000  0.000000  0.000000  
cpu2 : 0.000000  0.000000  0.000000  
cpu3 : 0.000000  0.000000  0.000000  
cpu4 : 0.000000  0.000000  0.000000  
cpu5 : 0.000000  0.000000  0.000000  
cpu6 : 0.000000  0.000000  0.000000  

			=========================
			= Beta Semaphore Checks =
			=========================


Note: We have threads waiting on beta semaphores !

Threads waiting on Beta Semaphores
==================================

                TICKS
WAITING OWNER   SINCE
TID     TID     RUN        CPU PRI SEMA ADDRESS   CALLER
------- ------- ---------- --- --- -------------- ----------
3985    -1      108494     0   646 0x5062160      shm_unattached_vmtotal+0x28
1727    -1      108465     0   639 0x5062160      shmget+0x44
3400627 -1      107438     0   646 0x5062160      shmget+0x44
3400629 -1      107329     0   646 0x5062160      shmget+0x44
3400631 -1      107031     0   646 0x5062160      shmget+0x44
3400633 -1      107021     0   646 0x5062160      shmget+0x44
3400635 -1      107009     0   646 0x5062160      shmget+0x44
3400637 -1      106998     0   646 0x5062160      shmget+0x44
3400649 -1      105110     0   646 0x5062160      shmget+0x44
3349351 -1      103089     0   646 0x5062160      shmdt+0x94
3349355 -1      103066     0   646 0x5062160      shmdt+0x94
3349353 -1      103051     0   646 0x5062160      shmdt+0x94
3349357 -1      103044     0   646 0x5062160      shmdt+0x94
3336307 -1      102754     0   646 0x5062160      shmdt+0x94
3336309 -1      102754     0   646 0x5062160      shmdt+0x94
3398914 -1      96215      0   646 0x5062160      shmdt+0x94
3400678 -1      96196      0   646 0x5062160      shmget+0x44
3400925 -1      76073      0   646 0x5062160      shmget+0x44
1848534 -1      72171      0   646 0x5062160      shmdt+0x94
3399350 -1      71081      0   646 0x5062160      shmdt+0x94
3399851 -1      57607      0   646 0x5062160      shmdt+0x94
1734209 -1      35144      0   646 0x5062160      shmdt+0x94
1849170 -1      32140      0   646 0x5062160      shmdt+0x94
3398538 -1      31071      0   646 0x5062160      shmdt+0x94
3371475 -1      27831      0   646 0x5062160      shmdt+0x94
3382826 -1      27699      0   646 0x5062160      shmdt+0x94
3382828 -1      27699      0   646 0x5062160      shmdt+0x94
3382824 -1      27699      0   646 0x5062160      shmdt+0x94
3371532 -1      25844      0   646 0x5062160      shmdt+0x94
3395643 -1      25838      0   646 0x5062160      shmdt+0x94
3377438 -1      25823      0   646 0x5062160      shmdt+0x94
3373554 -1      25801      0   646 0x5062160      shmdt+0x94
3372892 -1      25784      0   646 0x5062160      shmdt+0x94
3372756 -1      25756      0   646 0x5062160      shmdt+0x94
3372627 -1      25729      0   646 0x5062160      shmdt+0x94
3372440 -1      25705      0   646 0x5062160      shmdt+0x94
3371903 -1      25671      0   646 0x5062160      shmdt+0x94
3371753 -1      25644      0   646 0x5062160      shmdt+0x94
1848394 -1      21124      0   646 0x5062160      shmdt+0x94
3398651 -1      8316       0   646 0x5062160      shmdt+0x94
1847885 -1      8115       0   646 0x5062160      shmdt+0x94
3614    -1      108196     0   646 0x5101608      vmtotal+0x38
3979    -1      107267     0   646 0x5101608      vmtotal+0x38
1170    -1      106119     0   646 0x5101608      vmtotal+0x38
3620    -1      105824     0   646 0x5101608      vmtotal+0x38
3400653 -1      104577     0   646 0x5101608      vmtotal+0x38
3400657 -1      102251     0   646 0x5101608      vmtotal+0x38
3400673 -1      97918      0   646 0x5101608      vmtotal+0x38
3400687 -1      93401      0   646 0x5101608      vmtotal+0x38
3400732 -1      86840      0   646 0x5101608      vmtotal+0x38
3400797 -1      85858      0   646 0x5101608      vmtotal+0x38
3400824 -1      85220      0   646 0x5101608      vmtotal+0x38
3400918 -1      76727      0   646 0x5101608      vmtotal+0x38
3401011 -1      66469      0   646 0x5101608      vmtotal+0x38

			======================
			= Thread Information =
			======================

108 Threads ran in the last second
135 Threads ran in the last 5 seconds
178 Threads ran in the last 10 seconds
224 Threads ran in the last minute
545 Threads ran in the last hour

statdaemon ran 39 ticks ago


Most Common Wait Channels
=========================
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
shm_lock                                           41     108494     8115      
vx_worklist_thread_sv                              33     101        1         
memory_sleepers                                    21     51335      7031      
schedcpu_procp                                     16     792        79        
async_bufhead                                      16     3659120    3659120   
streams_mp_sync                                    15     4004       550       
per_processor_selects+0x40                         13     318402923  6         
vmtotal_lock                                       13     108196     66469     
per_processor_selects+0xc0                         13     318416287  2         
per_processor_selects+0x100                        12     318416245  62        
per_processor_selects+0x140                        10     318402999  0         
per_processor_selects+0x80                         9      318413364  6         
per_processor_selects                              9      77135280   1         
wait1(0x84022040)                                  8      318403214  318402098 
lvmkd_q                                            6      1500       1500      
LVM vg08/lv84                                      4      109726     107065    
per_processor_selects+0x180                        4      318403690  55798583  
svc_run(0x4044d180)                                3      318403686  59295944  
LVM vg10/lv28                                      3      108733     79208     
svc_run(0x4044d080)                                3      318403686  59295944  
LVM vg08/lv105                                     3      106577     106027    
svc_run(0x4044d100)                                3      318403686  59295944  
svc_run(0x4044d040)                                3      318403690  318403690 
svc_run(0x4044d1c0)                                3      318403686  59295944  
svc_run(0x4044d0c0)                                3      318403686  59294708  
svc_run(0x4044d140)                                3      318403686  59295944  
*uptr+0                                            3      51368      5         
wait1(0x9537c040)                                  3      68676      68676     
LVM vg03/lv1                                       2      105671     68743     
streams_blk_sync                                   2      318426433  318426433 
LVM vg11/lv164                                     2      109614     109601    
LVM vg06/lv84                                      2      109832     106225    
LVM vg04/lv84                                      2      107362     105251    
LVM vg07/lv87                                      2      95563      90727     
LVM vg09/lv210                                     2      109908     26697     
ticks_since_boot                                   2      39         39        
LVM vg08/lv86                                      2      109438     109144    
lock_write(0x88d2699e)                             2      108494     55880     


Most Common Sleep Callers
=========================
                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
hpstreams_read_int()                               500    318403203  550       
ksleep()                                           205    318415135  13        
lock_read()                                        72     109920     18312     
select()                                           44     318416287  1         
semop()                                            42     318424476  0         
vx_worklist_thread()                               33     101        1         
poll()                                             29     318416245  0         
real_nanosleep()                                   23     61288426   0         
reserve_freemem()                                  22     51335      7031      
sosleep()                                          22     318414527  547       
wait1()                                            21     318403214  51334     
svc_run()                                          21     318403690  59294708  
pm_sigwait()                                       20     318415138  9         
pset_schedcpu()                                    16     792        79        
async_daemon()                                     16     3659120    3659120   
streams_getmsg()                                   11     318413316  14408     
fifo_rdwr()                                        9      318413861  51334     
lvmkd_daemon()                                     6      1500       1500      
thread_wait()                                      4      318415126  318404069 
sigsuspend()                                       3      51368      5         
getmsg_subr()                                      2      85858      54709     
lock_write()                                       2      108494     55880     


Idle Globals
============

candidate_idle_spu = -1
migration_cycles = 1500000

Running Threads (TSRUNPROC) and idle Processors
===============================================


                    TICKS      TICKS                    I TICKS
                    SINCE      SINCE                    C SINCE      NREADY
TID     PID   PPID  RUN        IDLE       PRI SPU STATE S MIGR       FR LO AL COMMAND
------- ----- ----- ---------- ---------- --- --- ----- - ---------  -- -- -- -------
                               0              0   IDLE  N 1          0  0  0  
                               0              1   IDLE  N 1          0  0  0  
                               0              2   IDLE  N 13         0  0  0  
                               0              3   IDLE  N 1          0  0  0  
                               0              4   IDLE  N 1          0  0  0  
                               0              5   IDLE  N 1          0  0  0  
2       2     0     109928     109975     133 6   SYS   N 109975     0  1  0  vhand 

Note: 
FR: free to run on any processor (candidate for thread migration).
LO: locked (via processor affinity/mpctl) to this processor).
AL: Alpha semaphores misses (special scheduling when miss a sema).


Threads waiting on cpu (TSRUN) - sorted by cpu/pri/ticks-since-run
==================================================================

TID     PID   PPID  TICKS      PRI SPU BND GRP SYSCALL        COMMAND
------- ----- ----- ---------- --- --- --- --- -------------- -------
5344    4532  1     121544     154 6   Y   6   select         lpmc_em        

Note:  There is 1 thread in TSZOMB stat !


All Threads - sorted by ticks-since-run
=======================================

TID     PID   PPID  TICKS      PRI SPU STAT  SYSCALL        COMMAND        WCHAN
------- ----- ----- ---------- --- --- ----- -------------- -------------- -----
3983    3584  1     0          154 5   SLEEP poll           oracle         per_processor_selects+0x140
354788  3583  1     0          168 0   SLEEP nanosleep      rmiregistry    real_nanosleep(0x9b6ed4a8)
3400418 15667 1     0          156 4   SLEEP semop          oracle         __sem+0x11e4
354793  3583  1     1          168 3   SLEEP nanosleep      rmiregistry    real_nanosleep(0x93c574a8)
85      50    0     1          138 4   SLEEP n/a            vxfsd          vx_worklist_thread_sv
91      50    0     1          138 2   SLEEP n/a            vxfsd          vx_worklist_thread_sv
4009    3610  1     1          156 4   SLEEP semop          oracle         sema[28][18]+0x4
1030124 50    0     1          138 2   SLEEP n/a            vxfsd          vx_worklist_thread_sv
89      50    0     1          138 3   SLEEP n/a            vxfsd          vx_worklist_thread_sv
68      50    0     1          138 5   SLEEP n/a            vxfsd          vx_worklist_thread_sv
93      50    0     1          138 3   SLEEP n/a            vxfsd          vx_worklist_thread_sv
1830    1576  1     1          168 4   SLEEP nanosleep      prm3d          real_nanosleep(0x7bcb34a8)
90      50    0     1          138 4   SLEEP n/a            vxfsd          vx_worklist_thread_sv
92      50    0     1          138 4   SLEEP n/a            vxfsd          vx_worklist_thread_sv
69      50    0     1          138 4   SLEEP n/a            vxfsd          vx_worklist_thread_sv
86      50    0     1          138 4   SLEEP n/a            vxfsd          vx_worklist_thread_sv
3401114 50    0     1          138 4   SLEEP n/a            vxfsd          vx_worklist_thread_sv
77      50    0     1          138 3   SLEEP n/a            vxfsd          vx_worklist_thread_sv
79      50    0     1          138 3   SLEEP n/a            vxfsd          vx_worklist_thread_sv
3401115 50    0     1          138 5   SLEEP n/a            vxfsd          vx_worklist_thread_sv
66      50    0     1          138 5   SLEEP n/a            vxfsd          vx_worklist_thread_sv
72      50    0     1          138 5   SLEEP n/a            vxfsd          vx_worklist_thread_sv
95      50    0     1          138 1   SLEEP n/a            vxfsd          vx_worklist_thread_sv
67      50    0     1          138 1   SLEEP n/a            vxfsd          vx_worklist_thread_sv
65      50    0     1          138 1   SLEEP n/a            vxfsd          vx_event_wait(0x79c30a40)
3401118 50    0     1          138 5   SLEEP n/a            vxfsd          vx_worklist_thread_sv
73      50    0     1          138 5   SLEEP n/a            vxfsd          vx_worklist_thread_sv
1030125 50    0     1          138 2   SLEEP n/a            vxfsd          vx_worklist_thread_sv
88      50    0     1          138 4   SLEEP n/a            vxfsd          vx_worklist_thread_sv
3233    3045  3044  1          20  0   SLEEP select         cmcld          per_processor_selects
78      50    0     1          138 2   SLEEP n/a            vxfsd          vx_worklist_thread_sv
83      50    0     1          138 3   SLEEP n/a            vxfsd          vx_worklist_thread_sv
75      50    0     1          138 2   SLEEP n/a            vxfsd          vx_worklist_thread_sv
84      50    0     1          138 2   SLEEP n/a            vxfsd          vx_worklist_thread_sv
70      50    0     1          138 3   SLEEP n/a            vxfsd          vx_worklist_thread_sv
74      50    0     1          138 0   SLEEP n/a            vxfsd          vx_worklist_thread_sv
71      50    0     1          138 0   SLEEP n/a            vxfsd          vx_worklist_thread_sv
76      50    0     1          138 0   SLEEP n/a            vxfsd          vx_worklist_thread_sv
87      50    0     1          138 0   SLEEP n/a            vxfsd          vx_worklist_thread_sv
3580    3246  3244  2          154 3   SLEEP poll           prfAgent       per_processor_selects+0xc0
3400003 15268 1     4          156 0   SLEEP semop          oracle         __sem+0x14ac
3399453 14745 1     4          156 2   SLEEP semop          oracle         __sem+0x146c
3056216 16287 1     4          156 4   SLEEP semop          oracle         __sem+0x99c
3399348 14641 1     5          156 5   SLEEP semop          oracle         __sem+0xd54
1406    1300  1     5          120 1   SLEEP sigsuspend     xntpd          *uptr+0
3379563 26626 1     5          156 3   SLEEP semop          oracle         __sem+0xc14
3993    3594  1     6          154 2   SLEEP poll           oracle         per_processor_selects+0x80
3991    3592  1     6          154 1   SLEEP poll           oracle         per_processor_selects+0x40
1074073 29427 1     7          156 4   SLEEP semop          oracle         sema[28][204]+0x4
1018944 8645  1     7          156 4   SLEEP semop          oracle         __sem+0x117c
3055241 15583 1     7          156 5   SLEEP semop          oracle         sema[28][172]+0x4
1171050 27920 27917 9          168 3   SLEEP sigtimedwait   dbsnmp         pm_sigwait(0x791bf2c0)
3399997 15264 1     13         156 5   SLEEP semop          oracle         __sem+0x1444
1825    1576  1     13         154 0   SLEEP ksleep         prm3d          ksleep(0x798eb3c0)
3396439 11869 1     13         156 3   SLEEP semop          oracle         __sem+0xf14
2999300 28065 1     14         156 5   SLEEP semop          oracle         __sem+0xda4
1171049 27920 27917 14         168 3   SLEEP sigtimedwait   dbsnmp         pm_sigwait(0x791bf300)
3372718 20340 1     14         156 1   SLEEP semop          oracle         __sem+0xe3c
3370892 18666 1     14         156 4   SLEEP semop          oracle         sema[28][198]+0x4
3275    3074  3043  14         154 0   SLEEP ksleep         aws_orb        ksleep(0x798f8640)
5052    4247  1     15         168 0   SLEEP sigsuspend     xcomd          *uptr+0
3372658 20283 1     15         156 5   SLEEP semop          oracle         __sem+0xe74
1067685 23639 1     15         156 3   SLEEP semop          oracle         sema[28][143]+0x4
2998079 26891 1     18         156 5   SLEEP semop          oracle         sema[28][151]+0x4
1019205 0     0     20         156 2   SLEEP semop                         __sem+0x1184
3372662 20287 1     25         156 1   SLEEP semop          oracle         __sem+0xd94
3378851 25958 1     25         156 4   SLEEP semop          oracle         __sem+0x1284
3537    3223  3043  25         154 0   SLEEP ksleep         caiUxOs        ksleep(0x798d0b80)
1826    1700  1     27         154 1   SLEEP select         triggag        per_processor_selects+0x40
1018897 8601  1     27         156 5   SLEEP semop          oracle         __sem+0x1174
3371974 19664 1     31         156 1   SLEEP semop          oracle         __sem+0xd2c
4007    3608  1     31         156 5   SLEEP semop          oracle         sema[28][17]+0x4
3235    3043  1     32         154 2   SLEEP ksleep         awservices     ksleep(0x798f84c0)
3400076 15338 1     33         156 4   SLEEP semop          oracle         __sem+0x140c
4184    3770  1     34         156 1   SLEEP semop          oracle         sema[28][21]+0x4
3068334 27351 1     36         156 0   SLEEP semop          oracle         __sem+0x1244
0       0     0     39         127 1   SLEEP n/a            swapper        ticks_since_boot
4       4     0     39         128 2   SLEEP n/a            unhashdaemon   unhash
3       3     0     39         128 3   SLEEP n/a            statdaemon     ticks_since_boot
3357    3134  3131  42         168 4   SLEEP select         alarmgen       real_nanosleep(0x800d04a8)
3199843 22889 1     42         154 5   SLEEP poll           oracle         per_processor_selects+0x140
596     540   1     43         168 1   SLEEP sigtimedwait   vpard          pm_sigwait(0x78fb93c0)
4017    3618  1     44         156 0   SLEEP semop          oracle         sema[28][22]+0x4
3399334 14630 1     49         156 5   SLEEP semop          oracle         sema[28][38]+0x4
1456    1339  1     60         154 1   SLEEP select         lpsched        per_processor_selects+0x40
1504    0     0     62         154 4   SLEEP select                        per_processor_selects+0x100
3199877 22923 1     63         154 5   SLEEP poll           oracle         per_processor_selects+0x140
3596    3260  1     64         154 5   SLEEP poll           camf           per_processor_selects+0x140
1666    1548  1     65         -16 2   SLEEP ksleep         midaemon       ksleep(0x79a35080)
1665    1548  1     65         -16 1   SLEEP ki_call        midaemon       ki_cf
2952874 18500 1     66         156 2   SLEEP semop          oracle         sema[28][212]+0x4
3873    3476  1     66         154 0   SLEEP poll           caftf          per_processor_selects
3400280 15539 1     73         156 3   SLEEP semop          oracle         __sem+0x11c4
1018865 8569  1     75         156 3   SLEEP semop          oracle         __sem+0x116c
3435    3168  3043  76         154 2   SLEEP poll           aws_snmp       per_processor_selects+0x80
3600    3245  3043  77         154 4   SLEEP ksleep         proAgent       ksleep(0x798d0f00)
3420    3168  3043  77         154 0   SLEEP ksleep         aws_snmp       ksleep(0x79d72280)
3598    3245  3043  77         154 3   SLEEP ksleep         proAgent       ksleep(0x798d0e40)
53      27    0     79         128 5   SLEEP n/a            schedcpu       schedcpu_procp
1018092 7804  1     82         156 1   SLEEP semop          oracle         sema[28][118]+0x4
3562    3238  3043  85         154 2   SLEEP ksleep         hpaAgent       ksleep(0x798eb940)
3406    3148  3043  86         154 0   SLEEP ksleep         aws_sadmin     ksleep(0x798eb6c0)
3516    3213  3043  89         154 3   SLEEP ksleep         caiLogA2       ksleep(0x798d0940)
4030    3631  1     90         154 1   SLEEP poll           oracle         per_processor_selects+0x40
52      27    0     92         128 2   SLEEP n/a            schedcpu       schedcpu_procp
5027    4222  1     92         154 4   SLEEP select         seagent        per_processor_selects+0x100
3572    3244  3043  97         154 4   SLEEP ksleep         prfAgent       ksleep(0x79d72680)
707     644   643   97         127 3   SLEEP sigtimedwait   netfmt         pm_sigwait(0x791bf3c0)
82      50    0     101        138 1   SLEEP n/a            vxfsd          vx_worklist_thread_sv
80      50    0     101        138 0   SLEEP n/a            vxfsd          vx_worklist_thread_sv
4005    3606  1     131        156 0   SLEEP semop          oracle         sema[28][16]+0x4
1667    1548  1     142        -16 1   SLEEP nanosleep      midaemon       real_nanosleep(0x79d2b4a8)
4013    3614  1     154        156 3   SLEEP semop          oracle         sema[28][20]+0x4
3385923 2367  1     164        156 3   SLEEP semop          oracle         sema[28][121]+0x4
1476    1359  1     166        154 3   SLEEP select         diagmond       per_processor_selects+0xc0
5024    4219  1     166        1   5   SLEEP SEOS_syscall   seosd          have_request
3673    3238  3043  166        168 4   SLEEP sigtimedwait   hpaAgent       pm_sigwait(0x79471340)
3199849 22895 1     176        154 3   SLEEP poll           oracle         per_processor_selects+0xc0
51      27    0     179        128 5   SLEEP n/a            schedcpu       schedcpu_procp
50      27    0     192        128 1   SLEEP n/a            schedcpu       schedcpu_procp
2997971 26783 1     208        156 0   SLEEP semop          oracle         sema[28][91]+0x4
5250    4445  1     235        168 1   SLEEP sigtimedwait   seoswd         pm_sigwait(0x78fb9240)
3393356 9349  1     252        156 2   SLEEP semop          oracle         __sem+0xe9c
3820    3427  1     265        154 4   SLEEP ksleep         jre            ksleep(0x7939bd00)
3657    3244  3043  276        154 5   SLEEP ksleep         prfAgent       ksleep(0x8c4fd0c0)
49      27    0     279        128 1   SLEEP n/a            schedcpu       schedcpu_procp
48      27    0     292        128 0   SLEEP n/a            schedcpu       schedcpu_procp
1196    1131  1     295        154 4   SLEEP select         mib2agt        per_processor_selects+0x100
4093    3694  1359  313        158 1   SLEEP sigtimedwait   cclogd         pm_sigwait(0x78fb9100)
1712    1587  1     319        154 2   SLEEP select         swagentd       per_processor_selects+0x80
3573    3244  3043  376        154 0   SLEEP ksleep         prfAgent       ksleep(0x798d0d40)
47      27    0     379        128 1   SLEEP n/a            schedcpu       schedcpu_procp
46      27    0     392        128 5   SLEEP n/a            schedcpu       schedcpu_procp
27      27    0     479        128 1   SLEEP n/a            schedcpu       schedcpu_procp
45      27    0     492        128 0   SLEEP n/a            schedcpu       schedcpu_procp
1844    1576  1     518        154 0   SLEEP ksleep         prm3d          ksleep(0x79a351c0)
3835    3427  1     523        154 5   SLEEP ksleep         jre            ksleep(0x7939be40)
3365    2931  1559  547        154 5   SLEEP recvmsg        rep_server     sosleep(0x7909ec40)
3360    3131  1559  547        154 4   SLEEP ksleep         agdbserver     ksleep(0x7939b680)
3364    3134  3131  547        154 0   SLEEP recvmsg        alarmgen       sosleep(0x7909eb80)
3345    2931  1559  547        154 3   SLEEP ksleep         rep_server     ksleep(0x798d0780)
3366    3134  3131  547        154 0   SLEEP recvmsg        alarmgen       sosleep(0x84ff0a00)
3091210 18049 1     550        154 0   SLEEP read           oracle         STREAM 0x93d6b800
28      28    0     550        100 3   SLEEP n/a            smpsched       streams_mp_sync
59      27    0     579        128 1   SLEEP n/a            schedcpu       schedcpu_procp
58      27    0     592        128 1   SLEEP n/a            schedcpu       schedcpu_procp
57      27    0     679        128 3   SLEEP n/a            schedcpu       schedcpu_procp
3407    3148  3043  680        154 5   SLEEP ksleep         aws_sadmin     ksleep(0x798eb740)
3428    3148  3043  680        154 2   SLEEP ksleep         aws_sadmin     ksleep(0x798d0800)
4096    3697  1359  682        154 2   SLEEP select         psmctd         per_processor_selects+0x80
56      27    0     692        128 0   SLEEP n/a            schedcpu       schedcpu_procp
1576    1459  1     772        168 0   SLEEP sigtimedwait   acrecorderd    pm_sigwait(0x780b41c0)
55      27    0     779        128 1   SLEEP n/a            schedcpu       schedcpu_procp
54      27    0     792        128 3   SLEEP n/a            schedcpu       schedcpu_procp
4094    3695  1359  810        158 0   SLEEP read           diaglogd       diag2_sleep_flag
1687    1567  1459  814        168 5   SLEEP select         acrecorderd    real_nanosleep(0x79d7d4a8)
1552    1435  1     814        154 4   SLEEP poll           aclogrd        per_processor_selects+0x100
3343    2931  1559  830        154 1   SLEEP ksleep         rep_server     ksleep(0x79a35200)
3250    3057  3045  877        25  3   SLEEP select         cmsrvassistd   per_processor_selects+0xc0
3355    3131  1559  917        154 5   SLEEP ksleep         agdbserver     ksleep(0x79a352c0)
3659    3244  3043  928        154 4   SLEEP ksleep         prfAgent       ksleep(0x8c4fd140)
3658    3244  3043  928        154 2   SLEEP recv           prfAgent       sosleep(0x8009f7c0)
3660    3074  3043  928        154 1   SLEEP recv           aws_orb        sosleep(0x8009f880)
3671    3074  3043  928        154 2   SLEEP ksleep         aws_orb        ksleep(0x7939b8c0)
3517    3213  3043  986        154 0   SLEEP ksleep         caiLogA2       ksleep(0x798d09c0)
3626    3213  3043  986        154 4   SLEEP ksleep         caiLogA2       ksleep(0x8c4fd080)
3627    3074  3043  986        154 4   SLEEP recv           aws_orb        sosleep(0x79491dc0)
3624    3213  3043  986        154 1   SLEEP ksleep         caiLogA2       ksleep(0x798d0fc0)
42      42    0     986        100 4   SLEEP n/a            smpsched       streams_mp_sync
3625    3213  3043  986        154 4   SLEEP recv           caiLogA2       sosleep(0x79491f40)
3670    3074  3043  986        154 2   SLEEP ksleep         aws_orb        ksleep(0x7939b880)
31      31    0     986        100 3   SLEEP n/a            smpsched       streams_mp_sync
3838    3427  1     986        154 2   SLEEP ksleep         jre            ksleep(0x7939bec0)
3267    3073  3057  986        127 5   SLEEP select         cmgmsd         per_processor_selects+0x140
35      35    0     991        100 1   SLEEP n/a            smpsched       streams_mp_sync
30      30    0     991        100 3   SLEEP n/a            smpsched       streams_mp_sync
3813    3427  1     991        154 2   SLEEP ksleep         jre            ksleep(0x7939bc00)
1018852 8556  1     1070       154 5   SLEEP read           oracle         STREAM 0xadd7b100
41      41    0     1070       100 4   SLEEP n/a            smpsched       streams_mp_sync
3103    1243  1     1338       154 2   SLEEP ksleep         hpuxci         ksleep(0x79ac1100)
3347    1559  1     1368       154 4   SLEEP ksleep         perflbd        ksleep(0x798f8740)
23      23    0     1500       147 2   SLEEP n/a            lvmkd          lvmkd_q
25      25    0     1500       147 5   SLEEP n/a            lvmkd          lvmkd_q
22      22    0     1500       147 0   SLEEP n/a            lvmkd          lvmkd_q
21      21    0     1500       147 4   SLEEP n/a            lvmkd          lvmkd_q
24      24    0     1500       147 3   SLEEP n/a            lvmkd          lvmkd_q
20      20    0     1500       147 1   SLEEP n/a            lvmkd          lvmkd_q
37      37    0     1551       100 2   SLEEP n/a            smpsched       streams_mp_sync
1309    1234  1     1580       154 3   SLEEP ksleep         dmisp          ksleep(0x798eb040)
590     534   1     1777       154 0   SLEEP tsync          syncer         sync_proc_headers
3433    3074  3043  1849       154 1   SLEEP recv           aws_orb        sosleep(0x8009f4c0)
3431    3148  3043  1849       154 5   SLEEP ksleep         aws_sadmin     ksleep(0x798d0880)
3429    3148  3043  1849       154 5   SLEEP recv           aws_sadmin     sosleep(0x8009f400)
3427    3168  3043  1849       154 0   SLEEP ksleep         aws_snmp       ksleep(0x79a35480)
3432    3168  3043  1849       154 4   SLEEP ksleep         aws_snmp       ksleep(0x79a35500)
3421    3168  3043  1849       154 4   SLEEP ksleep         aws_snmp       ksleep(0x79d72300)
3895    3074  3043  1849       154 0   SLEEP ksleep         aws_orb        ksleep(0x798ebcc0)
3430    3168  3043  1849       154 4   SLEEP recv           aws_snmp       sosleep(0x793cbb80)
3617    3074  3043  1849       154 4   SLEEP ksleep         aws_orb        ksleep(0x798ebbc0)
3434    3074  3043  1849       154 2   SLEEP recv           aws_orb        sosleep(0x793cbc40)
4019    3620  1     1850       156 4   SLEEP semop          oracle         sema[28][23]+0x4
3341    2931  1559  1871       154 1   SLEEP ksleep         rep_server     ksleep(0x798d0680)
3276    3074  3043  2178       154 2   SLEEP ksleep         aws_orb        ksleep(0x798f86c0)
3295    3074  3043  2178       154 3   SLEEP ksleep         aws_orb        ksleep(0x7939b480)
669     608   1     2211       154 5   SLEEP select         syslogd        per_processor_selects+0x140
5266    4461  1     2422       154 3   SLEEP select         dm_stape       per_processor_selects+0xc0
1364    1260  1     2518       154 4   SLEEP ksleep         swci           ksleep(0x7939b240)
29      29    0     2552       100 5   SLEEP n/a            smpsched       streams_mp_sync
1171041 27920 27917 2736       154 1   SLEEP ksleep         dbsnmp         ksleep(0x798f8fc0)
39      39    0     2736       100 0   SLEEP n/a            smpsched       streams_mp_sync
38      38    0     2760       100 4   SLEEP n/a            smpsched       streams_mp_sync
1343    1247  1     3367       154 1   SLEEP ksleep         sdci           ksleep(0x798eb100)
34      34    0     3553       100 0   SLEEP n/a            smpsched       streams_mp_sync
32      32    0     3999       100 4   SLEEP n/a            smpsched       streams_mp_sync
40      40    0     3999       100 3   SLEEP n/a            smpsched       streams_mp_sync
33      33    0     4004       100 2   SLEEP n/a            smpsched       streams_mp_sync
36      36    0     4004       100 5   SLEEP n/a            smpsched       streams_mp_sync
3358    3131  1559  4897       154 3   SLEEP ksleep         agdbserver     ksleep(0x7939b600)
3361    3134  3131  4955       154 1   SLEEP ksleep         alarmgen       ksleep(0x79a35240)
3675    3238  3043  5178       154 4   SLEEP recv           hpaAgent       sosleep(0x78e3b340)
3680    3074  3043  5178       154 3   SLEEP ksleep         aws_orb        ksleep(0x7939b9c0)
3259    3065  3057  5407       25  1   SLEEP select         cmlogd         per_processor_selects+0x40
878     815   1     5604       154 4   SLEEP select         automount      per_processor_selects+0x100
3518    3213  3043  6250       154 4   SLEEP ksleep         caiLogA2       ksleep(0x798d0a00)
3563    3238  3043  6901       154 5   SLEEP ksleep         hpaAgent       ksleep(0x798eb9c0)
3247    3054  1     7031       130 5   SLEEP trap*          cmclconfd      memory_sleepers
1964536 8233  1     8020       154 1   SLEEP read           oracle         STREAM 0xb4ca3800
4590    3823  1     8020       154 4   SLEEP read           oracle         STREAM 0x8c845840
1847885 27737 1     8115       134 4   SLEEP shmdt          oracle         shm_lock
3676    3238  3043  8178       154 0   SLEEP ksleep         hpaAgent       ksleep(0x7939b980)
3677    3074  3043  8178       154 2   SLEEP recv           aws_orb        sosleep(0x78e3b4c0)
3398651 13966 1     8316       134 1   SLEEP shmdt          oracle         shm_lock
3367499 16211 1     9098       154 3   SLEEP read           oracle         STREAM 0x93997800
1847    1576  1     11353      154 2   SLEEP ksleep         prm3d          ksleep(0x798eb440)
1848    1576  1     11353      154 0   SLEEP poll           prm3d          per_processor_selects
1849    1576  1     12442      168 0   SLEEP nanosleep      prm3d          real_nanosleep(0x7bd0f4a8)
1827    1576  1     12524      154 2   SLEEP ksleep         prm3d          ksleep(0x798eb400)
3401116 3427  1     12906      178 2   ZOMB  lwp_detached_e jre            
3831    3427  1     14407      154 4   SLEEP ksleep         jre            ksleep(0x7939bd80)
3818    3427  1     14408      154 2   SLEEP accept         jre            STREAM 0x880790c0
1171043 27920 27917 16140      154 4   SLEEP ksleep         dbsnmp         ksleep(0xa620b040)
1937290 27920 27917 16140      154 3   SLEEP ksleep         dbsnmp         ksleep(0x8c4fd880)
1171042 27920 27917 16140      154 5   SLEEP ksleep         dbsnmp         ksleep(0x798f8f80)
1171062 27920 27917 16140      154 1   SLEEP ksleep         dbsnmp         ksleep(0x8c4fd840)
1391    1285  1     16408      64  2   SLEEP select         rbootd         per_processor_selects+0x80
3832    3427  1     17453      154 5   SLEEP ksleep         jre            ksleep(0x7939bdc0)
2998143 26955 1     18312      133 5   SLEEP read           oracle         LVM vg03/lv61
3370931 18705 1     19238      154 3   SLEEP read           oracle         STREAM 0x93b827c0
2998834 27639 1     19460      154 1   SLEEP read           oracle         STREAM 0x9b06e080
5040    4235  1     20211      138 2   SLEEP sigtimedwait   krsd           pm_sigwait(0x78fec300)
6245    1576  1     20211      168 5   SLEEP nanosleep      prm3d          real_nanosleep(0x7bd124a8)
1018842 8546  1     20748      154 3   SLEEP read           oracle         STREAM 0xadd58440
1848394 0     0     21124      134 1   SLEEP shmdt                         shm_lock
3363    3134  3131  23436      154 0   SLEEP ksleep         alarmgen       ksleep(0x79a35340)
3400313 50    0     24401      138 1   ZOMB  n/a            vxfsd          
3371753 19462 1     25644      134 3   SLEEP shmdt          oracle         shm_lock
3371903 19598 1     25671      134 4   SLEEP shmdt          oracle         shm_lock
3372440 20080 1     25705      134 5   SLEEP shmdt          oracle         shm_lock
3372627 20252 1     25729      134 2   SLEEP shmdt          oracle         shm_lock
3372756 20371 1     25756      134 2   SLEEP shmdt          oracle         shm_lock
3372892 20482 1     25784      134 3   SLEEP shmdt          oracle         shm_lock
3373554 21088 1     25801      134 0   SLEEP shmdt          oracle         shm_lock
3377438 24658 1     25823      134 3   SLEEP shmdt          oracle         shm_lock
3395643 11125 1     25838      134 1   SLEEP shmdt          oracle         shm_lock
3371532 19267 1     25844      134 5   SLEEP shmdt          oracle         shm_lock
2998147 26959 1     26697      133 5   SLEEP read           oracle         LVM vg09/lv210
3382826 29887 1     27699      134 4   SLEEP shmdt          oracle         shm_lock
3382824 29885 1     27699      134 3   SLEEP shmdt          oracle         shm_lock
3382828 29889 1     27699      134 0   SLEEP shmdt          oracle         shm_lock
4022    3623  1     27780      156 5   SLEEP semop          oracle         sema[28][24]+0x4
3371475 19212 1     27831      134 4   SLEEP shmdt          oracle         shm_lock
1073958 29324 1     27983      154 1   SLEEP read           oracle         STREAM 0xa34440c0
1073933 29300 1     28318      154 0   SLEEP read           oracle         STREAM 0x94cec440
1073528 28920 1     30035      130 1   SLEEP SEOS_execve    login          memory_sleepers
3695    3327  3223  30162      130 0   SLEEP trap*          cai_UxOs_ProcC memory_sleepers
3615    3223  3043  30996      154 3   SLEEP recv           caiUxOs        sosleep(0x7909e7c0)
3619    3074  3043  30996      154 1   SLEEP ksleep         aws_orb        ksleep(0x7939b700)
3470    3148  3043  30996      154 5   SLEEP poll           aws_sadmin     per_processor_selects+0x140
3398538 13868 1     31071      134 1   SLEEP shmdt          oracle         shm_lock
593     537   1     31618      168 5   SLEEP sigtimedwait   vphbd          pm_sigwait(0x79371240)
1849170 28664 1     32140      134 0   SLEEP shmdt          oracle         shm_lock
3372344 19992 1     33121      154 1   SLEEP read           oracle         STREAM 0x8caedbc0
1734209 21184 1     35144      134 4   SLEEP shmdt          oracle         shm_lock
5383    4571  1     35219      154 0   SLEEP select         sysstat_em     per_processor_selects
4165    3756  1     35980      130 3   SLEEP SEOS_fork      tnslsnr        memory_sleepers
3401113 16325 3756  35980      152 4   IDL   n/a            tnslsnr        
1461    1344  1     36477      130 1   SLEEP SEOS_fork      cron           memory_sleepers
3401112 16324 1344  36477      152 4   IDL   n/a            cron           
3401102 16316 2954  45136      130 4   SLEEP SEOS_execve    vxsvc          memory_sleepers
3251    2954  1     45136      152 0   SLEEP SEOS_execve    vxsvc          reserve_freemem(0xbcd42248)
3155    2954  1     45181      154 4   STOP  poll           vxsvc          
3340    2954  1     45314      154 3   STOP  poll           vxsvc          
5045    4240  1     46496      130 0   SLEEP trap*          p_client       memory_sleepers
4535    3778  3057  50688      130 5   SLEEP SEOS_fork      oracle9.sh     memory_sleepers
3401101 16315 3778  50688      152 2   IDL   n/a            oracle9.sh     
3401100 16314 4207  50692      152 2   IDL   n/a            inetd          
5012    4207  1     50692      130 2   SLEEP SEOS_fork      inetd          memory_sleepers
3401091 16305 1     51234      130 0   SLEEP trap*          glance         memory_sleepers
1416    1310  1     51235      130 2   SLEEP trap*          pwgrd          memory_sleepers
3401098 0     0     51333      130 1   SLEEP trap*                         memory_sleepers
3401093 16307 4207  51334      130 5   SLEEP trap*          telnetd        memory_sleepers
3401099 16313 16306 51334      130 0   SLEEP trap*          grep           memory_sleepers
3401095 16309 16306 51334      130 3   SLEEP trap*          ps             memory_sleepers
3401092 16306 4275  51334      158 5   SLEEP waitpid        sh             wait1(0xbaaa6040)
3401097 16311 16306 51334      154 3   SLEEP read           grep           FIFO 0x9335d240
3401096 16310 16306 51334      154 1   SLEEP read           grep           FIFO 0x88d16c80
3401094 16308 4207  51334      130 2   SLEEP trap*          telnetd        memory_sleepers
5080    4275  1     51335      158 3   SLEEP waitpid        dm_chassis     wait1(0x84d09040)
3401005 3682  1     51335      130 1   SLEEP trap*          java           memory_sleepers
3401081 16301 16285 51335      130 3   SLEEP trap*          ioscan         memory_sleepers
3401064 16283 16278 51335      130 2   SLEEP trap*          samx           memory_sleepers
3981    3582  1     51335      130 5   SLEEP trap*          oracle         memory_sleepers
3400926 3583  1     51335      130 0   SLEEP trap*          rmiregistry    memory_sleepers
1       1     0     51368      168 3   SLEEP sigsuspend     init           *uptr+0
1177    1112  1     53699      154 2   SLEEP select         snmpdm         per_processor_selects+0x80
3401083 14    0     54197      148 5   ZOMB  n/a            ioconfigd      
14      14    0     54623      152 1   SLEEP n/a            ioconfigd      SD$#var#tmp#ucAAAAAAa26803
3401066 16285 1     54626      158 4   SLEEP waitpid        ioparser.sh    wait1(0xbcd2d040)
3401059 16278 16154 54679      158 2   SLEEP waitpid        sam            wait1(0xafbdb040)
3400931 16154 16153 54709      158 1   SLEEP waitpid        ksh            wait1(0xbaa60040)
3400930 16153 4207  54709      154 0   SLEEP getmsg         telnetd        STREAM 0x9b13fc00
5077    4272  1     54935      154 0   SLEEP select         disk_em        per_processor_selects
3000097 28787 1     55880      133 2   SLEEP trap*          oracle         lock_write(0x88d2699e)
3399851 15128 1     57607      134 2   SLEEP shmdt          oracle         shm_lock
354804  3583  1     59468      154 5   SLEEP ksleep         rmiregistry    ksleep(0x798f8a00)
5079    4274  1     59715      154 3   SLEEP select         dm_TL_adapter  per_processor_selects+0xc0
5221    4416  1     61498      154 4   SLEEP select         dm_ses_enclosu per_processor_selects+0x100
5082    4277  1     61498      154 0   SLEEP select         dm_core_hw     per_processor_selects
5078    4273  1     61498      154 3   SLEEP select         dm_FCMS_adapte per_processor_selects+0x100
5161    4356  1     61498      154 4   SLEEP select         dm_memory      per_processor_selects+0x80
2998083 26895 1     63628      133 4   SLEEP read           oracle         LVM vg06/lv116
1536573 3682  1     65523      152 2   SLEEP SEOS_fork      java           thread_suspend_wait(0x93c2e0ed)
1536541 3682  1     65525      168 1   STOP  nanosleep      java           
1536535 3682  1     65526      154 5   STOP  ksleep         java           
3401011 16230 3238  66469      134 5   SLEEP pstat          hpacbcol       vmtotal_lock
3401014 16233 0     66478      152 1   IDL   n/a            java           
3400658 3682  1     68676      158 2   STOP  waitid         java           wait1(0x9537c040)
1536577 3682  1     68676      168 0   STOP  nanosleep      java           
3400674 3682  1     68676      158 5   STOP  waitid         java           wait1(0x9537c040)
3400654 3682  1     68676      158 3   STOP  waitid         java           wait1(0x9537c040)
3401004 16224 3682  68686      183 3   ZOMB  exit           df             
3399120 14423 1     68743      133 5   SLEEP read           oracle         LVM vg03/lv1
1536737 3682  1     68912      168 2   STOP  nanosleep      java           
2998138 26950 1     68946      133 5   SLEEP read           oracle         LVM vg10/lv159
1536563 3682  1     71081      154 5   STOP  ksleep         java           ksleep(0x79d724c0)
3400285 15543 1     71081      154 1   SLEEP read           oracle         STREAM 0xaf8be100
3400072 15334 1     71081      154 0   SLEEP read           oracle         STREAM 0x93917840
3399350 14643 1     71081      134 3   SLEEP shmdt          oracle         shm_lock
1536699 3682  1     71081      154 4   STOP  ksleep         java           
1536536 3682  1     71081      154 1   STOP  ksleep         java           ksleep(0x798ebe80)
1536537 3682  1     71081      154 3   STOP  ksleep         java           ksleep(0x798ebf80)
1305    1234  1     71110      154 2   SLEEP ksleep         dmisp          ksleep(0x7939b140)
1536552 3682  1     72163      168 1   STOP  nanosleep      java           
1848534 28090 1     72171      134 2   SLEEP shmdt          oracle         shm_lock
3399398 14690 1     73093      154 3   SLEEP read           oracle         STREAM 0x93674040
354883  3583  1     75230      154 4   SLEEP ksleep         rmiregistry    ksleep(0x79a35b40)
3400925 16151 1     76073      134 2   SLEEP shmget         oracle         shm_lock
1536704 3682  1     76073      154 3   STOP  read           java           STREAM 0xbc7e57c0
3400832 16062 4207  76502      154 1   SLEEP select         rlogind        per_processor_selects+0x40
3400920 3682  1     76679      178 2   ZOMB  lwp_detached_e java           
1536703 3682  1     76679      168 3   STOP  nanosleep      java           
1536534 3682  1     76698      154 3   STOP  ksleep         java           ksleep(0x798ebe40)
3400918 16145 16064 76727      134 0   SLEEP pstat          w              vmtotal_lock
3400834 16064 16062 76729      158 3   SLEEP waitpid        sh             wait1(0x90987040)
1536636 3682  1     78689      168 3   STOP  nanosleep      java           
3369801 17671 1     79208      133 2   SLEEP read           oracle         LVM vg10/lv28
354789  3583  1     81231      154 1   SLEEP ksleep         rmiregistry    ksleep(0x8c4fd480)
354790  3583  1     81231      154 4   SLEEP ksleep         rmiregistry    ksleep(0x8c4fd380)
3400702 3583  1     85034      178 4   ZOMB  lwp_detached_e rmiregistry    
3400824 16054 1     85220      134 1   SLEEP pstat          top            vmtotal_lock
3400797 16026 15958 85858      134 5   SLEEP pstat          top            vmtotal_lock
3400730 15957 4207  85858      154 0   SLEEP getmsg         telnetd        STREAM 0xafbe0880
3400731 15958 15957 85860      158 2   SLEEP waitpid        sh             wait1(0xbaa5e040)
3400732 15959 1     86840      134 3   SLEEP pstat          top            vmtotal_lock
2998675 27482 1     87314      133 4   SLEEP read           oracle         LVM vg05/lv87
1536697 3682  1     88026      154 1   STOP  accept         java           STREAM 0xaf8b87c0
354787  3583  1     88034      154 3   SLEEP ksleep         rmiregistry    ksleep(0x8c4fd440)
1536669 3682  1     88557      168 1   STOP  nanosleep      java           
3056357 16416 1     88866      133 1   SLEEP read           oracle         LVM vg03/lv64
2998679 27486 1     90727      133 0   SLEEP read           oracle         LVM vg07/lv87
3400687 15919 1     93401      134 1   SLEEP pstat          top            vmtotal_lock
1266130 26048 1     93681      133 3   SLEEP read           oracle         LVM vg07/lv27
2952878 18502 1     93762      133 4   SLEEP read           oracle         LVM vg06/lv29
2998130 26942 1     95332      133 3   SLEEP read           oracle         LVM vg06/lv86
3400678 15910 1     96196      134 3   SLEEP shmget         oracle         shm_lock
3398914 14225 1     96215      134 5   SLEEP shmdt          oracle         shm_lock
3538    3223  3043  97187      154 3   SLEEP ksleep         caiUxOs        ksleep(0x798d0c00)
3400673 15906 3682  97918      134 4   SLEEP pstat          vmstat         vmtotal_lock
1536567 3682  1     97927      154 3   STOP  read           java           FIFO 0x79218c40
3398951 14261 1     98217      154 2   SLEEP read           oracle         STREAM 0x93a724c0
3398945 14255 1     98218      154 1   SLEEP read           oracle         STREAM 0xbc680b80
3398949 14259 1     98218      154 3   SLEEP read           oracle         STREAM 0xafb72bc0
3398936 14246 1     98218      154 2   SLEEP read           oracle         STREAM 0xaf511c00
2453074 8638  1     98405      133 3   SLEEP read           oracle         LVM vg07/lv184
1018122 7834  1     98599      133 5   SLEEP read           oracle         LVM vg08/lv70
1018490 8201  1     99853      154 0   SLEEP read           oracle         STREAM 0x90a847c0
3394926 10767 1     99858      133 3   SLEEP read           oracle         LVM vg11/lv216
1018703 8409  1     99897      154 5   SLEEP read           oracle         STREAM 0xadbd2080
2453389 8924  1     100108     133 1   SLEEP read           oracle         LVM vg10/lv113
3400657 15896 3682  102251     134 1   SLEEP pstat          iostat         vmtotal_lock
1536580 3682  1     102259     154 4   STOP  read           java           FIFO 0x94f59800
3336309 20241 1     102754     134 0   SLEEP shmdt          oracle         shm_lock
3336307 20239 1     102754     134 1   SLEEP shmdt          oracle         shm_lock
3370383 18198 1     102882     154 1   SLEEP read           oracle         STREAM 0x8036c0c0
3599    3245  3043  102996     154 2   SLEEP ksleep         proAgent       ksleep(0x798d0ec0)
3349357 1022  1     103044     134 4   SLEEP shmdt          oracle         shm_lock
3349353 1018  1     103051     134 3   SLEEP shmdt          oracle         shm_lock
3349355 1020  1     103066     134 0   SLEEP shmdt          oracle         shm_lock
3349351 1016  1     103089     134 0   SLEEP shmdt          oracle         shm_lock
3669    3074  3043  104180     154 4   SLEEP ksleep         aws_orb        ksleep(0x7939b840)
3621    3245  3043  104180     154 2   SLEEP recv           proAgent       sosleep(0x78e3bb80)
1025781 14562 1     104471     133 4   SLEEP read           oracle         LVM vg02/lv34
1967025 10001 1     104526     133 5   SLEEP read           oracle         LVM vg06/lv11
3400653 15895 3682  104577     134 4   SLEEP pstat          vmstat         vmtotal_lock
1536564 3682  1     104587     154 1   STOP  read           java           FIFO 0x94f59500
1018193 7907  1     104690     133 2   SLEEP read           oracle         LVM vg10/lv28
3400649 0     0     105110     134 0   SLEEP shmget                        shm_lock
3148    2954  1     105156     154 1   STOP  ksleep         vxsvc          ksleep(0x798d0540)
3400612 15856 1     105251     133 1   SLEEP read           oracle         LVM vg04/lv84
2453227 8767  1     105297     133 2   SLEEP read           oracle         LVM vg02/lv39
1536678 3682  1     105671     154 1   STOP  read           java           STREAM 0x6fbdf100
3399630 14913 1     105671     133 4   SLEEP read           oracle         LVM vg03/lv1
1199700 24520 1     105778     133 4   SLEEP read           oracle         LVM vg05/lv66
3620    3245  3043  105824     134 0   SLEEP pstat          proAgent       vmtotal_lock
3622    3245  3043  105996     154 5   SLEEP ksleep         proAgent       ksleep(0x7939b7c0)
3623    3074  3043  105996     154 2   SLEEP recv           aws_orb        sosleep(0x78e3b580)
2335015 25838 1     106027     133 1   SLEEP read           oracle         LVM vg08/lv105
1018110 7822  1     106102     133 4   SLEEP read           oracle         LVM vg08/lv105
1170    1105  1     106119     134 5   SLEEP pstat          sendmail       vmtotal_lock
1019116 8861  1     106170     133 5   SLEEP read           oracle         LVM vg02/lv69
1018311 8025  1     106225     133 5   SLEEP read           oracle         LVM vg06/lv84
3373834 21357 1     106363     133 1   SLEEP read           oracle         LVM vg04/lv171
1077596 2685  1     106481     133 3   SLEEP read           oracle         LVM vg06/lv55
3379683 26765 1     106577     133 1   SLEEP read           oracle         LVM vg08/lv105
3400637 15880 1     106998     134 1   SLEEP shmget         oracle         shm_lock
3400635 0     0     107009     134 5   SLEEP shmget                        shm_lock
3400633 15876 1     107021     134 2   SLEEP shmget         oracle         shm_lock
3400631 15874 1     107031     134 3   SLEEP shmget         oracle         shm_lock
1018332 8046  1     107065     133 2   SLEEP read           oracle         LVM vg08/lv84
3979    3580  1     107267     134 0   SLEEP pstat          oracle         vmtotal_lock
3400629 15872 1     107329     134 4   SLEEP shmget         oracle         shm_lock
1018354 8068  1     107362     133 2   SLEEP read           oracle         LVM vg04/lv84
3400627 15870 1     107438     134 3   SLEEP shmget         oracle         shm_lock
1018307 8021  1     107441     133 2   SLEEP read           oracle         LVM vg07/lv29
3187712 12312 1     107461     133 0   SLEEP read           oracle         LVM vg07/lv86
2453391 8926  1     107917     133 5   SLEEP read           oracle         LVM vg09/lv53
3399226 14526 1     107979     133 4   SLEEP read           oracle         LVM vg02/lv105
3614    3223  3043  108196     134 5   SLEEP pstat          caiUxOs        vmtotal_lock
2998155 26967 1     108419     133 2   SLEEP read           oracle         LVM vg08/lv84
3396743 12154 1     108450     133 5   SLEEP read           oracle         LVM vg11/lv73
1727    1602  1     108465     127 4   SLEEP shmget         scopeux        shm_lock
3985    3586  1     108494     134 2   SLEEP pstat          oracle         shm_lock
3400624 15867 1     108494     133 1   SLEEP shmat          oracle         lock_write(0x88d2699e)
1018350 8064  1     108634     133 3   SLEEP read           oracle         LVM vg02/lv53
2452425 7897  1     108733     133 0   SLEEP read           oracle         LVM vg10/lv28
3399176 14477 1     108833     133 3   SLEEP read           oracle         LVM vg03/lv105
1074411 29749 1     108881     133 4   SLEEP read           oracle         LVM vg08/lv30
3367318 16047 1     108896     133 5   SLEEP read           oracle         LVM vg08/lv24
3336305 20237 1     108911     133 2   SLEEP read           oracle         LVM vg08/lv180
3616    3223  3043  109008     154 5   SLEEP ksleep         caiUxOs        ksleep(0x798ebb80)
3618    3074  3043  109008     154 2   SLEEP recv           aws_orb        sosleep(0x7909edc0)
3187716 12316 1     109144     133 1   SLEEP read           oracle         LVM vg08/lv86
3539    3223  3043  109189     154 4   SLEEP ksleep         caiUxOs        ksleep(0x798d0c40)
3195544 19474 1     109242     133 4   SLEEP read           oracle         LVM vg08/lv5
3379859 26973 1     109342     133 2   SLEEP read           oracle         LVM vg09/lv87
3379131 26221 1     109403     133 5   SLEEP read           oracle         LVM vg03/lv162
1018848 8552  1     109438     133 0   SLEEP read           oracle         LVM vg08/lv86
3999    3600  1     109601     133 3   SLEEP write          oracle         LVM vg11/lv164
3997    3598  1     109614     133 3   SLEEP write          oracle         LVM vg11/lv164
3400446 15697 1     109673     133 5   SLEEP read           oracle         LVM vg07/lv103
1074409 29747 1     109702     133 2   SLEEP read           oracle         LVM vg08/lv84
3400616 15860 1     109715     133 2   SLEEP read           oracle         LVM vg03/lv86
1077594 2683  1     109726     133 3   SLEEP read           oracle         LVM vg08/lv84
3995    3596  1     109744     133 3   SLEEP write          oracle         LVM vg02/lv192
1018346 8060  1     109832     133 5   SLEEP read           oracle         LVM vg06/lv84
3397019 12425 1     109868     133 1   SLEEP read           oracle         LVM vg06/lv106
3400141 15401 1     109872     133 2   SLEEP read           oracle         LVM vg08/lv102
4003    3604  1     109878     133 0   SLEEP write          oracle         LVM vg11/lv166
2998159 26971 1     109883     133 1   SLEEP read           oracle         LVM vg09/lv83
3372702 20325 1     109887     133 2   SLEEP read           oracle         LVM vg05/lv52
3394255 10155 1     109899     133 4   SLEEP read           oracle         LVM vg02/lv84
3396709 12120 1     109905     133 4   SLEEP read           oracle         LVM vg03/lv87
3399950 15217 1     109905     133 4   SLEEP read           oracle         LVM vg03/lv104
3397335 12729 1     109906     133 5   SLEEP read           oracle         LVM vg05/lv107
3399589 14876 1     109906     133 5   SLEEP read           oracle         LVM vg09/lv103
3399404 14696 1     109906     133 3   SLEEP read           oracle         LVM vg05/lv104
3379214 26303 1     109908     133 0   SLEEP read           oracle         LVM vg09/lv210
3400603 15847 1     109916     133 3   SLEEP read           oracle         LVM vg09/lv102
3397712 13082 1     109920     133 5   SLEEP read           oracle         LVM vg01/lv25
TID     PID   PPID  TICKS      PRI SPU STAT  SYSCALL        COMMAND        WCHAN
------- ----- ----- ---------- --- --- ----- -------------- -------------- -----
2       2     0     109928     133 6   RUN   n/a            vhand          
3370850 18624 1     111504     154 5   SLEEP read           oracle         STREAM 0x6fbe7100
1081491 6180  1     112661     154 5   SLEEP read           oracle         STREAM 0xaf5c8080
3371843 19544 1     117490     154 2   SLEEP read           oracle         STREAM 0x94d72080
1071034 26556 1     117533     154 6   SLEEP read           oracle         STREAM 0x91ab7100
3371885 19584 1     117561     154 3   SLEEP read           oracle         STREAM 0x93ace100
1018513 8221  1     117696     154 1   SLEEP read           oracle         STREAM 0x90a2b880
3372613 20242 1     117752     154 6   SLEEP read           oracle         STREAM 0x93b110c0
3437    3168  3043  118188     154 3   SLEEP poll           aws_snmp       per_processor_selects+0xc0
TID     PID   PPID  TICKS      PRI SPU STAT  SYSCALL        COMMAND        WCHAN
------- ----- ----- ---------- --- --- ----- -------------- -------------- -----
5344    4532  1     121544     154 6   RUN   select         lpmc_em        
1018494 8205  1     131292     154 2   SLEEP read           oracle         STREAM 0x95b720c0
4103    3696  1359  131519     154 4   SLEEP ksleep         memlogd        ksleep(0x79d727c0)
4106    3696  1359  131521     154 3   SLEEP select         memlogd        per_processor_selects+0xc0
4109    3696  1359  131521     154 0   SLEEP ksleep         memlogd        ksleep(0x8c4fd1c0)
4108    3696  1359  131521     154 0   SLEEP ksleep         memlogd        ksleep(0x8c4fd180)
4111    3696  1359  131521     154 0   SLEEP ksleep         memlogd        ksleep(0x8c4fd240)
4110    3696  1359  131521     154 0   SLEEP ksleep         memlogd        ksleep(0x8c4fd200)
4105    3696  1359  131521     154 4   SLEEP ksleep         memlogd        ksleep(0x79d72800)
3371847 19547 1     138459     154 6   SLEEP read           oracle         STREAM 0x9375e7c0
1536624 3682  1     138940     168 1   STOP  nanosleep      java           
1171048 27920 27917 139899     154 6   SLEEP ksleep         dbsnmp         ksleep(0xa620b180)
1536599 3682  1     140165     168 3   STOP  nanosleep      java           
1548012 14327 1     141814     154 4   SLEEP read           oracle         STREAM 0xa8df3400
1632708 25949 1     141818     154 0   SLEEP read           oracle         STREAM 0xaf565c40
1378    1272  1     158944     168 4   SLEEP sigtimedwait   scrdaemon      pm_sigwait(0x794710c0)
3376025 23359 1     161277     154 1   SLEEP read           oracle         STREAM 0x96404b80
3371096 18860 1     162243     154 4   SLEEP read           oracle         STREAM 0x93233440
1018299 8013  1     168366     154 6   SLEEP read           oracle         STREAM 0x95b4b840
3376005 23341 1     168789     154 6   SLEEP read           oracle         STREAM 0xaf8b8400
1453    1247  1     172708     168 5   SLEEP sigtimedwait   sdci           pm_sigwait(0x79371140)
4011    3612  1     176817     156 3   SLEEP semop          oracle         sema[28][19]+0x4
2958326 23017 1     179650     154 1   SLEEP read           oracle         STREAM 0xb53547c0
1018303 8017  1     180779     154 0   SLEEP read           oracle         STREAM 0x95f1f040
1850000 29408 1     181695     154 2   SLEEP read           oracle         STREAM 0xaf565880
3374232 21723 1     184627     154 0   SLEEP read           oracle         STREAM 0x9b25f480
3398848 14159 1     185643     154 4   SLEEP read           oracle         STREAM 0x94741c00
1070002 25576 1     187686     154 5   SLEEP read           oracle         STREAM 0x9378ac00
1018521 8229  1     193959     154 4   SLEEP read           oracle         STREAM 0x91adabc0
1018143 7855  1     195678     154 3   SLEEP read           oracle         STREAM 0x959d2040
3000944 29541 1     197186     154 0   SLEEP read           oracle         STREAM 0x90aba800
3374669 22120 1     197201     154 0   SLEEP read           oracle         STREAM 0x9390a040
2998804 27609 1     197207     154 5   SLEEP read           oracle         STREAM 0xaf9847c0
1536656 3682  1     198725     168 0   STOP  nanosleep      java           
1536647 3682  1     198727     168 6   STOP  nanosleep      java           
2998587 27394 1     212700     154 4   SLEEP read           oracle         STREAM 0x91793080
1018044 7756  1     239891     154 4   SLEEP read           oracle         STREAM 0x90aba440
1303    1234  1     255421     154 3   SLEEP ksleep         dmisp          ksleep(0x7939b0c0)
2998812 27617 1     315565     154 4   SLEEP read           oracle         STREAM 0x937fa0c0
2998808 27613 1     316182     154 1   SLEEP read           oracle         STREAM 0x936267c0
1536635 3682  1     319761     168 2   STOP  nanosleep      java           
1018509 8217  1     320435     154 1   SLEEP read           oracle         STREAM 0xa6216400
1018505 8213  1     320493     154 5   SLEEP read           oracle         STREAM 0x95b72840
3374228 21719 1     331269     154 2   SLEEP read           oracle         STREAM 0xaf7eac00
1171052 27920 27917 333153     154 6   SLEEP ksleep         dbsnmp         ksleep(0x79a35b80)
3371779 19484 1     387289     154 4   SLEEP read           oracle         STREAM 0xbc7a2100
2974347 6694  1     414813     154 4   SLEEP read           oracle         STREAM 0x9b1dd800
3386738 3171  1     425264     154 6   SLEEP read           oracle         STREAM 0x9381fbc0
813     750   1     431897     154 0   SLEEP poll           rpcbind        per_processor_selects
1018362 8076  1     450021     154 6   SLEEP read           oracle         STREAM 0x93bd9bc0
3374285 21771 1     474279     154 5   SLEEP read           oracle         STREAM 0xb0a28480
3374218 21710 1     476866     154 1   SLEEP read           oracle         STREAM 0x9afef400
3378911 26016 1     508663     154 2   SLEEP read           oracle         STREAM 0xadc9d100
3392260 8294  1     583572     154 2   SLEEP read           oracle         STREAM 0xaf6d6bc0
3372323 19971 1     686110     154 6   SLEEP read           oracle         STREAM 0xafc0b4c0
1522621 20917 1     853521     154 1   SLEEP read           oracle         STREAM 0xaf60b4c0
1879635 25575 1     916173     154 3   SLEEP read           oracle         FIFO 0x77fe4c40
1018274 7988  1     988668     154 5   SLEEP read           oracle         STREAM 0x915ab800
1072876 0     0     1117028    154 6   SLEEP read                          STREAM 0x93d6bbc0
1072935 28358 1     1118710    154 5   SLEEP read           oracle         STREAM 0xab81bc00
3382198 29305 1     1122661    154 4   SLEEP read           oracle         STREAM 0x93a67c40
1018060 7772  1     1129055    154 6   SLEEP read           oracle         STREAM 0x947af800
3373796 21320 1     1130342    154 1   SLEEP read           oracle         STREAM 0x93538100
3383365 241   1     1146228    154 2   SLEEP read           oracle         STREAM 0xaf9ee480
3386608 3037  1     1156384    154 0   SLEEP read           oracle         STREAM 0x949eb880
3385931 2373  1     1160003    154 5   SLEEP read           oracle         STREAM 0x6fbe7c40
2997991 26803 1     1160626    154 5   SLEEP read           oracle         STREAM 0xafbc0080
2997995 26807 1     1168833    154 2   SLEEP read           oracle         STREAM 0xaf5c1440
2978369 10478 1     1182961    154 4   SLEEP read           oracle         STREAM 0xb4db7040
3367256 15992 1     1241178    154 4   SLEEP read           oracle         STREAM 0x9b2b9480
3374070 21575 1     1253647    154 3   SLEEP read           oracle         STREAM 0xaf5110c0
3385151 1683  1     1282774    154 0   SLEEP read           oracle         STREAM 0xadd667c0
3385107 1639  1     1282832    154 5   SLEEP read           oracle         STREAM 0x9aedcc40
1018358 8072  1     1352978    154 2   SLEEP read           oracle         STREAM 0x94d5ab80
3359    3131  1559  1354942    154 2   SLEEP select         agdbserver     per_processor_selects+0x80
3362    3134  3131  1355391    154 2   SLEEP ksleep         alarmgen       ksleep(0x798eb640)
3354    3131  1559  1355391    154 1   SLEEP ksleep         agdbserver     ksleep(0x798eb600)
3382989 131   1     1392374    154 0   SLEEP read           oracle         STREAM 0xafbce040
3371889 19587 1     1408104    154 0   SLEEP read           oracle         STREAM 0xaf518480
3372074 19756 1     1437527    154 4   SLEEP read           oracle         STREAM 0xbc061800
1018366 8080  1     1450627    154 6   SLEEP read           oracle         STREAM 0x9b01f480
3382219 29318 1     1452812    154 4   SLEEP read           oracle         STREAM 0x93917c00
3382210 29311 1     1453986    154 4   SLEEP read           oracle         STREAM 0x9372e7c0
3382212 29313 1     1454359    154 2   SLEEP read           oracle         STREAM 0x8ce80440
3367    3131  1559  1550896    154 5   SLEEP ksleep         agdbserver     ksleep(0x798f8800)
3380029 27184 1     1599127    154 2   SLEEP read           oracle         STREAM 0x93bf0400
3379760 26857 1     1603967    154 0   SLEEP read           oracle         STREAM 0xade63c40
3379911 27038 1     1605784    154 1   SLEEP read           oracle         STREAM 0xade8a400
3379718 26806 1     1625490    154 0   SLEEP read           oracle         STREAM 0x9345b040
3379570 26632 1     1632703    154 4   SLEEP read           oracle         STREAM 0xb08ea0c0
3089724 16660 1     1658435    154 1   SLEEP read           oracle         STREAM 0x9390a7c0
3089728 16664 1     1661494    154 0   SLEEP read           oracle         STREAM 0xafc0b100
1948827 23401 1     1672483    154 3   SLEEP read           oracle         STREAM 0x93a00840
1181525 7925  1     1672556    154 3   SLEEP read           oracle         STREAM 0x93ca2480
2998163 26975 1     1697957    154 6   SLEEP read           oracle         STREAM 0x939a1040
2998257 27069 1     1703230    154 6   SLEEP read           oracle         STREAM 0x8cf96bc0
2998167 26979 1     1740140    154 3   SLEEP read           oracle         STREAM 0x93bcb040
3377714 24910 1     1770150    154 3   SLEEP read           oracle         STREAM 0x93770100
3825    3131  1559  1781282    154 6   SLEEP ksleep         agdbserver     ksleep(0x798f8cc0)
1079297 4417  1     1808278    154 2   SLEEP read           oracle         STREAM 0xa5318880
3376673 23960 1     1825781    154 4   SLEEP read           oracle         STREAM 0xbc061bc0
3057833 17734 1     1839599    154 4   SLEEP read           oracle         STREAM 0x8cd7a880
2997943 26755 1     1839639    154 3   SLEEP read           oracle         STREAM 0xaf86f7c0
2998574 27381 1     1839647    154 6   SLEEP read           oracle         STREAM 0x93504bc0
3057839 17739 1     1842940    154 3   SLEEP read           oracle         STREAM 0x94afebc0
1018064 7776  1     1878443    154 2   SLEEP read           oracle         STREAM 0x94f3dc00
2998261 27073 1     1884732    154 5   SLEEP read           oracle         STREAM 0x84ebd480
3376382 23702 1     1888298    154 6   SLEEP read           oracle         STREAM 0xb08da800
1018130 7842  1     2074130    154 5   SLEEP read           oracle         STREAM 0x94cd70c0
1018671 8377  1     2090028    154 0   SLEEP read           oracle         STREAM 0x9b251880
1089852 14130 1     2090822    154 5   SLEEP read           oracle         STREAM 0xaf716100
3372480 20117 1     2180751    154 1   SLEEP read           oracle         STREAM 0x84dfd800
3372605 20232 1     2196011    154 2   SLEEP read           oracle         STREAM 0xaf5a9100
3372098 19774 1     2242090    154 4   SLEEP read           oracle         STREAM 0xade90840
3196822 20333 1     2427672    154 3   SLEEP read           oracle         STREAM 0x6fbdf880
2987354 17398 1     2428025    154 1   SLEEP read           oracle         STREAM 0xaddf2400
2987339 17384 1     2432106    154 6   SLEEP read           oracle         STREAM 0x93ca2840
3367275 16009 1     2616714    154 4   SLEEP read           oracle         STREAM 0xafa6c4c0
26      26    0     2870791    148 6   SLEEP n/a            lvmschedd      lv_schedule_daemon
852     789   1     3659120    154 6   SLEEP async_daemon   biod           async_bufhead
838     775   1     3659120    154 6   SLEEP async_daemon   biod           async_bufhead
850     787   1     3659120    154 1   SLEEP async_daemon   biod           async_bufhead
847     784   1     3659120    154 1   SLEEP async_daemon   biod           async_bufhead
846     783   1     3659120    154 5   SLEEP async_daemon   biod           async_bufhead
851     788   1     3659120    154 5   SLEEP async_daemon   biod           async_bufhead
845     782   1     3659120    154 5   SLEEP async_daemon   biod           async_bufhead
848     785   1     3659120    154 2   SLEEP async_daemon   biod           async_bufhead
840     777   1     3659120    154 4   SLEEP async_daemon   biod           async_bufhead
837     774   1     3659120    154 2   SLEEP async_daemon   biod           async_bufhead
843     780   1     3659120    154 4   SLEEP async_daemon   biod           async_bufhead
839     776   1     3659120    154 4   SLEEP async_daemon   biod           async_bufhead
842     779   1     3659120    154 3   SLEEP async_daemon   biod           async_bufhead
841     778   1     3659120    154 4   SLEEP async_daemon   biod           async_bufhead
849     786   1     3659120    154 3   SLEEP async_daemon   biod           async_bufhead
844     781   1     3659120    154 6   SLEEP async_daemon   biod           async_bufhead
2711435 24856 4207  3967878    154 4   SLEEP poll           rpc.cmsd       per_processor_selects+0x100
1536696 3682  1     5191448    168 6   STOP  nanosleep      java           real_nanosleep(0x7c1ee4a8)
1188    1123  1     7288493    154 3   SLEEP select         hp_unixagt     per_processor_selects+0xc0
1210    1145  1     7288498    154 0   SLEEP select         cmsnmpd        per_processor_selects
1670022 28342 1     7820385    154 5   SLEEP read           oracle         STREAM 0xaf663840
1147573 6579  1     8108141    154 2   SLEEP read           oracle         STREAM 0xadbe37c0
1171046 27920 27917 8156103    154 1   SLEEP ksleep         dbsnmp         ksleep(0xa620b100)
2998651 27458 1     9275348    154 3   SLEEP read           oracle         STREAM 0x90a33bc0
3287869 9309  1     9277677    154 1   SLEEP read           oracle         STREAM 0xaf6630c0
1018249 7963  1     9822890    154 2   SLEEP read           oracle         STREAM 0x8cb5d400
1959325 27920 27917 9826355    154 6   SLEEP ksleep         dbsnmp         ksleep(0x8cebe140)
1171051 27920 27917 9842213    154 4   SLEEP accept         dbsnmp         STREAM 0x90aaf480
1171047 27920 27917 9842277    154 3   SLEEP accept         dbsnmp         STREAM 0xade847c0
3189020 13495 1     9999602    154 5   SLEEP read           oracle         STREAM 0x9aedc4c0
1536659 3682  1     10016008   168 6   STOP  nanosleep      java           real_nanosleep(0x84d744a8)
2457826 12555 1     10107771   154 0   SLEEP read           oracle         STREAM 0x9130f100
1018159 7871  1     10111616   154 2   SLEEP read           oracle         STREAM 0x93c34bc0
1536545 3682  1     14925715   154 5   STOP  ksleep         java           ksleep(0x798f89c0)
12      12    0     15897127   -32 3   SLEEP n/a            ttisr          ttirr
3260    3066  3057  15897127   20  6   SLEEP select         cmlvmd         per_processor_selects+0x40
3369    3136  0     15897127   20  2   SLEEP n/a            slvm_sorcvd    hpstreams_read_int(0x800658e8)
3368    3135  0     15919180   20  3   SLEEP n/a            resyncd        slvm_resync_q
1938334 14200 1     16563099   154 3   SLEEP read           oracle         STREAM 0xb09f8c00
3189685 14106 1     16802426   154 3   SLEEP read           oracle         STREAM 0x84808c00
3195272 19221 1     16897453   154 5   SLEEP read           oracle         STREAM 0xade2e080
3191380 15675 1     17260256   154 3   SLEEP read           oracle         STREAM 0xaf6fe0c0
3191376 15671 1     17260311   154 1   SLEEP read           oracle         STREAM 0xb5354b80
3190045 14429 1     17355761   154 6   SLEEP read           oracle         STREAM 0x81226100
3189792 14207 1     17381462   154 0   SLEEP read           oracle         STREAM 0xbc9d9c00
3189788 14203 1     17381511   154 3   SLEEP read           oracle         STREAM 0x9b2b9840
3189573 14007 1     17394282   154 6   SLEEP read           oracle         STREAM 0x937fa840
3189567 14002 1     17394328   154 0   SLEEP read           oracle         STREAM 0x8cdc3100
2998151 26963 1     17437562   154 4   SLEEP read           oracle         STREAM 0xaf86f040
2998003 26815 1     18370032   154 1   SLEEP read           oracle         STREAM 0x95c124c0
3166706 22508 1     18699522   154 6   SLEEP read           oracle         STREAM 0x939a1b80
3166716 22514 1     18699542   154 6   SLEEP read           oracle         STREAM 0x93845c40
1640693 3203  1     18815140   154 1   SLEEP read           oracle         STREAM 0xaf7f9400
1018830 8534  1     18817564   154 4   SLEEP read           oracle         STREAM 0xadd36080
1182085 8588  1     19356572   154 0   SLEEP read           oracle         STREAM 0xa53acb80
2270500 27920 27917 24984393   154 2   SLEEP ksleep         dbsnmp         ksleep(0x79ac1480)
1018482 8193  1     25007487   154 1   SLEEP read           oracle         STREAM 0x93115040
1018599 8308  1     25814349   154 4   SLEEP read           oracle         STREAM 0x9b152100
3066273 25334 1     26134962   154 6   SLEEP read           oracle         STREAM 0x8814b080
3080184 8119  1     26256737   154 4   SLEEP read           oracle         STREAM 0xade68100
1018838 8542  1     26473150   154 4   SLEEP read           oracle         STREAM 0xadd2cb80
1093627 17484 1     26999418   154 3   SLEEP read           oracle         STREAM 0x9b25f840
2998796 27601 1     33473937   154 3   SLEEP read           oracle         STREAM 0x937e8840
2998792 27597 1     33473987   154 0   SLEEP read           oracle         STREAM 0x9b395bc0
2998770 27576 1     33474268   154 4   SLEEP read           oracle         STREAM 0x93942100
2998766 27572 1     33474315   154 6   SLEEP read           oracle         STREAM 0x9384ac40
2998762 27568 1     33474365   154 5   SLEEP read           oracle         STREAM 0x937e8c00
2998756 27563 1     33474415   154 6   SLEEP read           oracle         STREAM 0x937a5bc0
2998752 27559 1     33474463   154 0   SLEEP read           oracle         STREAM 0x9384a880
2998748 27555 1     33474513   154 4   SLEEP read           oracle         STREAM 0x93c42440
2998744 27551 1     33474561   154 6   SLEEP read           oracle         STREAM 0x93b82040
2998740 27547 1     33474608   154 6   SLEEP read           oracle         STREAM 0x94d117c0
2998736 27543 1     33474657   154 0   SLEEP read           oracle         STREAM 0x93551840
2998732 27539 1     33474712   154 4   SLEEP read           oracle         STREAM 0x94adf100
2998728 27535 1     33474760   154 6   SLEEP read           oracle         STREAM 0x933d6bc0
2998724 27531 1     33474809   154 1   SLEEP read           oracle         STREAM 0x9381f800
2998720 27527 1     33474859   154 3   SLEEP read           oracle         STREAM 0x937a5440
2998716 27523 1     33474912   154 6   SLEEP read           oracle         STREAM 0x933d6800
2998712 27519 1     33474963   154 5   SLEEP read           oracle         STREAM 0x938f2800
2998708 27515 1     33475011   154 3   SLEEP read           oracle         STREAM 0x938e2480
2998704 27511 1     33475058   154 5   SLEEP read           oracle         STREAM 0x9387db80
2998700 27507 1     33475110   154 3   SLEEP read           oracle         STREAM 0x90a33080
2998696 27503 1     33475158   154 4   SLEEP read           oracle         STREAM 0x93673400
2998691 27498 1     33475206   154 0   SLEEP read           oracle         STREAM 0x938f2440
2998687 27494 1     33475253   154 0   SLEEP read           oracle         STREAM 0xaddf27c0
2998683 27490 1     33475306   154 5   SLEEP read           oracle         STREAM 0x9345b7c0
2998663 27470 1     33475547   154 1   SLEEP read           oracle         STREAM 0x93663c00
2998659 27466 1     33475598   154 6   SLEEP read           oracle         STREAM 0x93d3f4c0
2998641 27448 1     33475745   154 0   SLEEP read           oracle         STREAM 0x90a33440
2998637 27444 1     33475797   154 5   SLEEP read           oracle         STREAM 0x909c6b80
2998631 27438 1     33475853   154 1   SLEEP read           oracle         STREAM 0x9356ec00
2998619 27426 1     33475933   154 6   SLEEP read           oracle         STREAM 0x9363e4c0
2998582 27389 1     33476038   154 3   SLEEP read           oracle         STREAM 0x935f8bc0
2998578 27385 1     33476087   154 2   SLEEP read           oracle         STREAM 0x935bd4c0
2998570 27377 1     33476185   154 3   SLEEP read           oracle         STREAM 0x935bd100
2998566 27373 1     33476233   154 1   SLEEP read           oracle         STREAM 0x93535480
2998562 27369 1     33476282   154 4   SLEEP read           oracle         STREAM 0x947b5480
2998558 27365 1     33476333   154 4   SLEEP read           oracle         STREAM 0x93351bc0
2998554 27361 1     33476383   154 3   SLEEP read           oracle         STREAM 0x935297c0
2998550 27357 1     33476436   154 6   SLEEP read           oracle         STREAM 0x9113d800
2998546 27353 1     33476490   154 5   SLEEP read           oracle         STREAM 0xaf768840
2998542 27349 1     33476538   154 6   SLEEP read           oracle         STREAM 0x93351080
2998537 27345 1     33476587   154 3   SLEEP read           oracle         STREAM 0x91d97b80
2998531 27341 1     33476635   154 4   SLEEP read           oracle         STREAM 0x93504080
2998527 27337 1     33476684   154 2   SLEEP read           oracle         STREAM 0x935350c0
2998521 27332 1     33476735   154 6   SLEEP read           oracle         STREAM 0x91585840
2998517 27328 1     33476784   154 1   SLEEP read           oracle         STREAM 0x931157c0
2998513 27324 1     33476829   154 0   SLEEP read           oracle         STREAM 0x94648880
2998509 27320 1     33476879   154 4   SLEEP read           oracle         STREAM 0x81113800
2998505 27316 1     33476925   154 0   SLEEP read           oracle         STREAM 0x93555bc0
2998501 0     0     33476971   154 3   SLEEP read                          STREAM 0x95f0fbc0
2998488 27299 1     33477019   154 5   SLEEP read           oracle         STREAM 0x81008c40
2998484 27295 1     33477069   154 1   SLEEP read           oracle         STREAM 0x93388400
2998480 27291 1     33477116   154 3   SLEEP read           oracle         STREAM 0x94bd7440
2998476 27287 1     33477164   154 5   SLEEP read           oracle         STREAM 0x933004c0
2998472 27283 1     33477214   154 0   SLEEP read           oracle         STREAM 0x8490ac00
2998468 27279 1     33477265   154 0   SLEEP read           oracle         STREAM 0x8c513840
2998464 27275 1     33477315   154 1   SLEEP read           oracle         STREAM 0x95998040
2998460 27271 1     33477363   154 2   SLEEP read           oracle         STREAM 0x930a1800
2998456 27267 1     33477416   154 1   SLEEP read           oracle         STREAM 0x91291400
2998452 27263 1     33477464   154 0   SLEEP read           oracle         STREAM 0x849dab80
2998448 27259 1     33477515   154 1   SLEEP read           oracle         STREAM 0x96e63400
2998444 27255 1     33477565   154 5   SLEEP read           oracle         STREAM 0x95c08880
2998440 27251 1     33477615   154 0   SLEEP read           oracle         STREAM 0x93555080
2998436 27247 1     33477663   154 5   SLEEP read           oracle         STREAM 0x849da7c0
2998432 27243 1     33477711   154 1   SLEEP read           oracle         STREAM 0x95c61880
2998428 27239 1     33477757   154 1   SLEEP read           oracle         STREAM 0x94a54c00
2998424 27235 1     33477808   154 5   SLEEP read           oracle         STREAM 0x946484c0
2998418 27230 1     33477869   154 2   SLEEP read           oracle         STREAM 0x93115400
2998414 27226 1     33477921   154 4   SLEEP read           oracle         STREAM 0x95439480
2998410 27222 1     33477969   154 1   SLEEP read           oracle         STREAM 0x84931840
2998406 27218 1     33478019   154 1   SLEEP read           oracle         STREAM 0x95815080
2998402 27214 1     33478066   154 4   SLEEP read           oracle         STREAM 0x95f94800
2998398 27210 1     33478114   154 5   SLEEP read           oracle         STREAM 0x9588e040
2998394 27206 1     33478160   154 0   SLEEP read           oracle         STREAM 0x95583840
2998389 27201 1     33478212   154 5   SLEEP read           oracle         STREAM 0x947b50c0
2998385 27197 1     33478264   154 2   SLEEP read           oracle         STREAM 0x90a84b80
2998381 27193 1     33478311   154 1   SLEEP read           oracle         STREAM 0x95ade800
2998377 27189 1     33478357   154 4   SLEEP read           oracle         STREAM 0x908d3b80
2998373 27185 1     33478404   154 6   SLEEP read           oracle         STREAM 0x79a1f800
2998369 27181 1     33478453   154 2   SLEEP read           oracle         STREAM 0x9b09a080
2998365 0     0     33478503   154 1   SLEEP read                          STREAM 0x90a51480
2998361 27173 1     33478553   154 6   SLEEP read           oracle         STREAM 0x949eb100
2998357 27169 1     33478600   154 2   SLEEP read           oracle         STREAM 0x91ada440
2998353 27165 1     33478647   154 5   SLEEP read           oracle         STREAM 0x8caed800
2998349 27161 1     33478692   154 0   SLEEP read           oracle         STREAM 0x91257800
2998345 27157 1     33478744   154 4   SLEEP read           oracle         STREAM 0x957b0880
2998341 27153 1     33478792   154 5   SLEEP read           oracle         STREAM 0x90a51840
2998337 27149 1     33478842   154 6   SLEEP read           oracle         STREAM 0x9334d400
2998333 27145 1     33478893   154 2   SLEEP read           oracle         STREAM 0x949ebc40
2998329 27141 1     33478941   154 4   SLEEP read           oracle         STREAM 0x84ae8c40
2998325 27137 1     33478988   154 3   SLEEP read           oracle         STREAM 0x94c91880
2998321 27133 1     33479036   154 1   SLEEP read           oracle         STREAM 0x81135840
2998317 27129 1     33479083   154 3   SLEEP read           oracle         STREAM 0x95743480
2998313 27125 1     33479132   154 2   SLEEP read           oracle         STREAM 0x910dd4c0
2998309 27121 1     33479182   154 1   SLEEP read           oracle         STREAM 0x908d3400
2998305 27117 1     33479231   154 6   SLEEP read           oracle         STREAM 0x93c34080
2998301 27113 1     33479277   154 0   SLEEP read           oracle         STREAM 0x84820c40
2998297 27109 1     33479326   154 4   SLEEP read           oracle         STREAM 0x8cf96800
2998293 27105 1     33479376   154 1   SLEEP read           oracle         STREAM 0x9ae68480
2998289 27101 1     33479433   154 2   SLEEP read           oracle         STREAM 0x9af3d040
2998285 27097 1     33479479   154 6   SLEEP read           oracle         STREAM 0x94c71c00
2998281 27093 1     33479529   154 0   SLEEP read           oracle         STREAM 0x95afc0c0
2998267 27079 1     33479631   154 6   SLEEP n/a            oracle         STREAM 0x94da3b80
2998265 27077 1     33479676   154 6   SLEEP read           oracle         STREAM 0x9b298100
2998253 27065 1     33479829   154 2   SLEEP read           oracle         STREAM 0xaf97f080
2998249 27061 1     33479877   154 3   SLEEP read           oracle         STREAM 0x95998400
2998245 27057 1     33479927   154 6   SLEEP read           oracle         STREAM 0x954e9b80
2998239 27051 1     33479977   154 6   SLEEP read           oracle         STREAM 0x94a12c40
2998235 27047 1     33480026   154 5   SLEEP read           oracle         STREAM 0x8c845c00
2998231 27043 1     33480075   154 5   SLEEP read           oracle         STREAM 0x79446040
2998227 27039 1     33480125   154 5   SLEEP read           oracle         STREAM 0x94cd7840
2998223 27035 1     33480173   154 3   SLEEP read           oracle         STREAM 0x93d8f480
2998219 27031 1     33480221   154 1   SLEEP read           oracle         STREAM 0x94cd7c00
2998215 27027 1     33480269   154 0   SLEEP read           oracle         STREAM 0x84ad0b80
2998211 27023 1     33480319   154 4   SLEEP read           oracle         STREAM 0x9334db80
2998207 27019 1     33480368   154 5   SLEEP read           oracle         STREAM 0x8cf0c080
2998203 27015 1     33480414   154 4   SLEEP read           oracle         STREAM 0x954e1800
2998199 27011 1     33480465   154 3   SLEEP read           oracle         STREAM 0x8cfc8480
2998195 27007 1     33480514   154 0   SLEEP read           oracle         STREAM 0x9579f840
2998191 27003 1     33480566   154 5   SLEEP read           oracle         STREAM 0x957b0100
2998187 26999 1     33480615   154 6   SLEEP read           oracle         STREAM 0x8cf3ac00
2998175 26987 1     33480763   154 4   SLEEP read           oracle         STREAM 0x8cf3a0c0
2998171 26983 1     33480810   154 3   SLEEP read           oracle         STREAM 0x91625100
2998134 26946 1     33481279   154 0   SLEEP read           oracle         STREAM 0x7cc98bc0
2998126 26938 1     33481408   154 3   SLEEP read           oracle         STREAM 0x954e97c0
2998122 26934 1     33481456   154 4   SLEEP read           oracle         STREAM 0x95a630c0
2998118 26930 1     33481572   154 3   SLEEP read           oracle         STREAM 0x957b8040
2998114 26926 1     33481619   154 6   SLEEP read           oracle         STREAM 0xaf8b8040
2998110 26922 1     33481674   154 1   SLEEP read           oracle         STREAM 0xaf76fc40
2998106 26918 1     33481720   154 0   SLEEP n/a            oracle         STREAM 0x8cf96080
2998102 26914 1     33481767   154 5   SLEEP read           oracle         STREAM 0xb08f8080
2998098 26910 1     33481815   154 1   SLEEP read           oracle         STREAM 0x9312b480
2998094 26906 1     33481860   154 5   SLEEP read           oracle         STREAM 0x9b04db80
2998090 26902 1     33481908   154 5   SLEEP read           oracle         STREAM 0xaf5c1800
2998073 26885 1     33482096   154 2   SLEEP read           oracle         STREAM 0x79414800
2998067 26880 1     33482145   154 6   SLEEP read           oracle         STREAM 0x84adc800
2998059 26872 1     33482196   154 1   SLEEP read           oracle         STREAM 0x9b01f0c0
2998047 26860 1     33482274   154 3   SLEEP n/a            oracle         STREAM 0x9b617080
2998039 26852 1     33482328   154 6   SLEEP read           oracle         STREAM 0x795730c0
2998035 26848 1     33482380   154 2   SLEEP read           oracle         STREAM 0x84a8abc0
2998031 26844 1     33482426   154 6   SLEEP read           oracle         STREAM 0x9ae08100
2998027 26840 1     33482474   154 0   SLEEP read           oracle         STREAM 0x848c9b80
2998023 26835 1     33482523   154 6   SLEEP read           oracle         STREAM 0x84880c40
2998019 26831 1     33482569   154 0   SLEEP read           oracle         STREAM 0x8ce80bc0
2998015 26827 1     33482618   154 1   SLEEP read           oracle         STREAM 0x953664c0
2998011 26823 1     33482666   154 6   SLEEP read           oracle         STREAM 0x849667c0
2998007 26819 1     33482713   154 1   SLEEP read           oracle         STREAM 0x8cef2b80
2997987 26799 1     33482924   154 6   SLEEP read           oracle         STREAM 0x8ce23880
2997967 26779 1     33483165   154 6   SLEEP read           oracle         STREAM 0xafbc0800
2997963 26775 1     33483213   154 6   SLEEP read           oracle         STREAM 0x95b72480
2997959 26771 1     33483263   154 4   SLEEP read           oracle         STREAM 0x8ce297c0
2997955 26767 1     33483308   154 1   SLEEP read           oracle         STREAM 0x8ce23100
2997951 26763 1     33483358   154 4   SLEEP read           oracle         STREAM 0x8cfe2c00
2997947 26759 1     33483406   154 5   SLEEP read           oracle         STREAM 0x9b182400
2997935 26747 1     33483545   154 1   SLEEP read           oracle         STREAM 0x959987c0
2997923 26734 1     33483684   154 4   SLEEP read           oracle         STREAM 0x911b8880
2997919 26730 1     33483733   154 3   SLEEP read           oracle         STREAM 0x8cb5d7c0
2997915 26726 1     33483783   154 2   SLEEP read           oracle         STREAM 0xaf518c00
2997911 26722 1     33483830   154 6   SLEEP read           oracle         STREAM 0x8ce29b80
2997907 26718 1     33483879   154 1   SLEEP read           oracle         STREAM 0x95c61c40
2997903 26714 1     33483924   154 1   SLEEP read           oracle         STREAM 0xaf97fbc0
2997899 26710 1     33483972   154 4   SLEEP read           oracle         STREAM 0xafb79c00
2997895 26706 1     33484020   154 4   SLEEP read           oracle         STREAM 0x9b04d040
2997891 26702 1     33484068   154 6   SLEEP read           oracle         STREAM 0x9cade440
2997883 26694 1     33484178   154 0   SLEEP read           oracle         STREAM 0xafa054c0
2997878 26689 1     33484226   154 0   SLEEP read           oracle         STREAM 0x8ceb9c40
2997874 26685 1     33484278   154 6   SLEEP read           oracle         STREAM 0x84a8dc40
2997870 26681 1     33484325   154 3   SLEEP read           oracle         STREAM 0xafe017c0
2997866 26677 1     33484375   154 4   SLEEP read           oracle         STREAM 0x9119b480
2997862 26673 1     33484419   154 0   SLEEP read           oracle         STREAM 0xaf663480
2997858 26669 1     33484470   154 4   SLEEP read           oracle         STREAM 0xaf67fc00
2997854 26665 1     33484519   154 1   SLEEP read           oracle         STREAM 0xaf4ef800
2997848 26659 1     33484570   154 2   SLEEP read           oracle         STREAM 0x9b0df080
2997840 26651 1     33484623   154 4   SLEEP read           oracle         STREAM 0xaf53bc40
2997836 26647 1     33484669   154 3   SLEEP read           oracle         STREAM 0xa3467c00
2997832 26643 1     33484718   154 1   SLEEP read           oracle         STREAM 0xba9267c0
2997828 26639 1     33484766   154 2   SLEEP read           oracle         STREAM 0xba9c7480
2997824 26635 1     33484814   154 5   SLEEP read           oracle         STREAM 0x8ce80800
2997820 26631 1     33484861   154 6   SLEEP read           oracle         STREAM 0x7cc34840
2997816 26627 1     33484909   154 5   SLEEP read           oracle         STREAM 0x8cf75c00
2997812 26623 1     33484957   154 3   SLEEP read           oracle         STREAM 0xafd6e080
2997325 26191 1     33524703   154 4   SLEEP read           oracle         STREAM 0x7990e4c0
1249    1184  1     55789551   154 3   SLEEP getsockname    dced           STREAM 0xb5354400
1311    1234  1     55796584   154 5   SLEEP ksleep         dmisp          ksleep(0x798d00c0)
3342    2931  1559  55797291   154 2   SLEEP ksleep         rep_server     ksleep(0x798d0700)
1337    1234  1     55797583   154 5   SLEEP ksleep         dmisp          ksleep(0x798eb0c0)
1310    1234  1     55797584   154 1   SLEEP ksleep         dmisp          ksleep(0x798eb080)
3344    2931  1559  55798583   154 6   SLEEP select         rep_server     per_processor_selects+0x180
1312    1234  1     55798584   154 3   SLEEP select         dmisp          per_processor_selects+0xc0
1313    1234  1     55798584   154 2   SLEEP ksleep         dmisp          LVM vg22/lv19
3824    2931  1559  55810307   154 3   SLEEP ksleep         rep_server     ksleep(0x798f8c80)
1372    1234  1     55810307   154 3   SLEEP ksleep         dmisp          ksleep(0x798d01c0)
1349    1234  1     55838904   154 2   SLEEP ksleep         dmisp          ksleep(0x7939b2c0)
861     798   1     59290751   154 5   SLEEP poll           rpc.statd      per_processor_selects+0x140
3160    2973  1     59290754   154 3   SLEEP poll           rpc.mountd     per_processor_selects+0xc0
3189    3002  2995  59294708   154 2   SLEEP nfssvc         nfsd           svc_run(0x4044d0c0)
3187    3000  2991  59295944   154 3   SLEEP nfssvc         nfsd           svc_run(0x4044d100)
3194    3007  2989  59295944   154 4   SLEEP nfssvc         nfsd           svc_run(0x4044d140)
3180    2993  2989  59295944   154 6   SLEEP nfssvc         nfsd           svc_run(0x4044d1c0)
3188    3001  2992  59295944   154 1   SLEEP nfssvc         nfsd           svc_run(0x4044d080)
3181    2994  2989  59295944   154 5   SLEEP nfssvc         nfsd           svc_run(0x4044d180)
3186    2999  2995  59295944   154 2   SLEEP nfssvc         nfsd           svc_run(0x4044d0c0)
2687382 3035  1     59575362   154 6   SLEEP read           oracle         STREAM 0x91625c40
2687394 3050  1     59575362   154 6   SLEEP read           oracle         STREAM 0x94d5a400
1018370 8084  1     59687167   154 5   SLEEP read           oracle         STREAM 0x908d37c0
2687405 3060  1     59703479   154 5   SLEEP read           oracle         STREAM 0x9b2b1440
2687398 3055  1     59706265   154 6   SLEEP read           oracle         STREAM 0xb4dc3400
2687390 3046  1     59706430   154 1   SLEEP read           oracle         STREAM 0x952e57c0
2687409 3064  1     59707087   154 6   SLEEP read           oracle         STREAM 0xaf6f3bc0
1018241 7955  1     59948521   154 2   SLEEP read           oracle         STREAM 0x9115f080
1018126 7838  1     60731814   154 4   SLEEP read           oracle         STREAM 0x94cbec40
2687386 3039  1     60869652   154 2   SLEEP read           oracle         STREAM 0xb09ff100
2687378 3031  1     60869843   154 4   SLEEP read           oracle         STREAM 0x954e9400
2687374 3027  1     60869943   154 5   SLEEP read           oracle         STREAM 0x93c34800
2687370 3023  1     60870038   154 2   SLEEP read           oracle         STREAM 0x959d2400
354760  3583  1     61288426   168 5   SLEEP nanosleep      rmiregistry    real_nanosleep(0x9b1054a8)
1018474 8185  1     68917874   154 4   SLEEP read           oracle         STREAM 0x9334d040
1018675 8381  1     70955485   154 3   SLEEP read           oracle         STREAM 0xab812800
3302    3098  3078  77135280   154 0   SLEEP select         dtlogin        per_processor_selects
1943338 18254 1     94294809   154 6   SLEEP read           oracle         STREAM 0x955bd080
1526525 3043  1     95155946   154 1   SLEEP ksleep         awservices     ksleep(0x79ac1640)
3239    3043  1     95155947   154 4   SLEEP accept         awservices     STREAM 0x7bff5c00
3821    3427  1     103652885  154 2   SLEEP ksleep         jre            ksleep(0x7939bd40)
1976672 18612 1     120364574  154 1   SLEEP read           oracle         STREAM 0x9b0d9040
1879637 25578 1     121703998  154 0   SLEEP read           oracle         FIFO 0x94f59380
1018551 8259  1     122696346  154 0   SLEEP read           oracle         STREAM 0x95544c00
354798  3583  1     128549009  154 4   SLEEP ksleep         rmiregistry    ksleep(0x8c4fd580)
1536544 3682  1     128819136  154 3   STOP  ksleep         java           ksleep(0x798f8e40)
1191368 17127 1     139957040  154 3   SLEEP read           oracle         STREAM 0xaf91ac00
1018245 7959  1     139958489  154 1   SLEEP read           oracle         STREAM 0x94cec080
1018179 7891  1     140161256  154 4   SLEEP read           oracle         STREAM 0x95f94440
1536701 3583  1     156565031  154 5   SLEEP ksleep         rmiregistry    ksleep(0x7939b5c0)
1536510 3682  1     156617478  154 0   STOP  ksleep         java           ksleep(0x798ebec0)
1536698 3682  1     156617491  154 1   STOP  ksleep         java           ksleep(0x79a35bc0)
1536551 3682  1     156622224  154 4   STOP  ksleep         java           ksleep(0x79ac1340)
1536543 3682  1     156622261  154 1   STOP  ksleep         java           ksleep(0x798f8e80)
1536542 3682  1     156622261  154 4   STOP  ksleep         java           ksleep(0x798f8ec0)
3814    3427  1     191226897  154 4   SLEEP ksleep         jre            ksleep(0x7939bc40)
1171003 27920 27917 191300961  154 3   SLEEP ksleep         dbsnmp         ksleep(0x79ac1600)
3828    3427  1     191301648  154 3   SLEEP accept         jre            STREAM 0x880e9b80
1171004 27920 27917 191301740  154 3   SLEEP select         dbsnmp         per_processor_selects+0xc0
1171000 27917 1     191301749  158 5   SLEEP waitpid        dbsnmpwd       wait1(0xaf741040)
1018427 8140  1     205679511  154 0   SLEEP read           oracle         STREAM 0x7cc304c0
1018834 8538  1     206354361  154 5   SLEEP read           oracle         STREAM 0xadd2c040
1018826 8530  1     206354544  154 5   SLEEP read           oracle         STREAM 0xadcc9c40
1018806 8510  1     206355031  154 1   SLEEP read           oracle         STREAM 0x959ab100
1018802 8506  1     206355132  154 6   SLEEP read           oracle         STREAM 0xadcc1880
1018798 8502  1     206355237  154 2   SLEEP read           oracle         STREAM 0xadc75840
1018794 8498  1     206355330  154 3   SLEEP read           oracle         STREAM 0xadc75480
1018790 8494  1     206355423  154 3   SLEEP read           oracle         STREAM 0xadc7c080
1018786 8490  1     206355511  154 3   SLEEP read           oracle         STREAM 0xa52d7800
1018782 8486  1     206355616  154 2   SLEEP read           oracle         STREAM 0x9b2e3b80
1018776 8482  1     206355720  154 1   SLEEP read           oracle         STREAM 0xadcc1100
1018766 8472  1     206355824  154 0   SLEEP read           oracle         STREAM 0xadc4fc00
1018757 8463  1     206355915  154 5   SLEEP read           oracle         STREAM 0xadc9d4c0
1018747 8453  1     206356004  154 1   SLEEP read           oracle         STREAM 0xadbd6bc0
1018741 8447  1     206356097  154 1   SLEEP read           oracle         STREAM 0xadc137c0
1018737 8443  1     206356187  154 1   SLEEP read           oracle         STREAM 0xadc13040
1018733 8439  1     206356289  154 4   SLEEP read           oracle         STREAM 0xadc3ebc0
1018729 8435  1     206356380  154 6   SLEEP read           oracle         STREAM 0xadb09480
1018725 8431  1     206356480  154 6   SLEEP read           oracle         STREAM 0xadc43100
1018721 8427  1     206356562  154 6   SLEEP read           oracle         STREAM 0xab812440
1018715 8421  1     206356656  154 1   SLEEP read           oracle         STREAM 0xadbd6800
1018711 8417  1     206356743  154 3   SLEEP read           oracle         STREAM 0xadbd2800
1018707 8413  1     206356836  154 4   SLEEP read           oracle         STREAM 0xa345e880
1018699 8405  1     206357014  154 3   SLEEP read           oracle         STREAM 0xadc0c480
1018695 8401  1     206357104  154 1   SLEEP read           oracle         STREAM 0xadc0c0c0
1018691 8397  1     206357185  154 0   SLEEP read           oracle         STREAM 0x95f94bc0
1018687 8393  1     206357272  154 4   SLEEP read           oracle         STREAM 0xadbb6bc0
1018683 8389  1     206357365  154 4   SLEEP read           oracle         STREAM 0x914db040
1018679 8385  1     206357442  154 0   SLEEP read           oracle         STREAM 0xadbb6440
1018667 8373  1     206357715  154 4   SLEEP read           oracle         STREAM 0x95878480
1018662 8368  1     206357800  154 6   SLEEP read           oracle         STREAM 0x9b26a040
1018658 8364  1     206357881  154 2   SLEEP read           oracle         STREAM 0x947afbc0
1018654 8360  1     206357996  154 6   SLEEP read           oracle         STREAM 0x9b395440
1018649 8356  1     206358095  154 3   SLEEP read           oracle         STREAM 0x9118c880
1018643 8352  1     206358181  154 0   SLEEP read           oracle         STREAM 0xab812080
1018639 8348  1     206358268  154 4   SLEEP read           oracle         STREAM 0x95439c00
1018635 8344  1     206358367  154 5   SLEEP read           oracle         STREAM 0xadba6040
1018628 8337  1     206358454  154 0   SLEEP read           oracle         STREAM 0x930a1080
1018624 8333  1     206358556  154 4   SLEEP read           oracle         STREAM 0xadb9d100
1018620 8329  1     206358644  154 1   SLEEP read           oracle         STREAM 0xadb7fc00
1018616 8325  1     206358750  154 2   SLEEP n/a            oracle         STREAM 0x9b5e3c00
1018612 8321  1     206358837  154 3   SLEEP read           oracle         STREAM 0x95330100
1018607 8316  1     206358921  154 5   SLEEP read           oracle         STREAM 0x9b5e3840
1018603 8312  1     206359006  154 1   SLEEP read           oracle         STREAM 0xadb7f480
1018595 8304  1     206359196  154 4   SLEEP read           oracle         STREAM 0x9b27b480
1018591 8300  1     206359288  154 6   SLEEP read           oracle         STREAM 0x94ab4c00
1018587 8296  1     206359450  154 6   SLEEP read           oracle         STREAM 0xadb7f0c0
1018583 8292  1     206359536  154 6   SLEEP read           oracle         STREAM 0x848dd400
1018579 8288  1     206359625  154 5   SLEEP read           oracle         STREAM 0x94f3d840
1018575 8283  1     206359744  154 0   SLEEP read           oracle         STREAM 0x95d74880
1018571 8279  1     206359831  154 3   SLEEP read           oracle         STREAM 0x90a17880
1018567 8275  1     206359938  154 4   SLEEP read           oracle         STREAM 0x94bd7800
1018563 8271  1     206360047  154 4   SLEEP read           oracle         STREAM 0x9b617800
1018559 8267  1     206360148  154 3   SLEEP read           oracle         STREAM 0x9b2f0b80
1018555 8263  1     206360230  154 2   SLEEP read           oracle         STREAM 0xa3444480
1018547 8255  1     206360424  154 0   SLEEP read           oracle         STREAM 0x959ab4c0
1018543 8251  1     206360512  154 4   SLEEP read           oracle         STREAM 0x813e7480
1018539 8247  1     206360612  154 6   SLEEP read           oracle         STREAM 0x95878840
1018535 8243  1     206360704  154 2   SLEEP read           oracle         STREAM 0x813e7c00
1018531 8239  1     206360790  154 0   SLEEP read           oracle         STREAM 0xadb47080
1018517 8225  1     206360959  154 2   SLEEP read           oracle         STREAM 0x952f2480
1018500 8209  1     206361301  154 3   SLEEP read           oracle         STREAM 0x94cd7480
1018486 8197  1     206361566  154 5   SLEEP read           oracle         STREAM 0x88fdcc00
1018469 8180  1     206361914  154 1   SLEEP read           oracle         STREAM 0x94a8f040
1018465 8176  1     206361996  154 4   SLEEP read           oracle         STREAM 0x90814440
1018461 8172  1     206362085  154 1   SLEEP read           oracle         STREAM 0xa53ac040
1018455 8166  1     206362164  154 0   SLEEP read           oracle         STREAM 0x94d5a040
1018449 8160  1     206362255  154 6   SLEEP read           oracle         STREAM 0x9b16d480
1018437 8150  1     206362343  154 1   SLEEP read           oracle         STREAM 0x954e9040
1018419 8132  1     206362784  154 6   SLEEP read           oracle         STREAM 0x909c67c0
1018415 8128  1     206362871  154 4   SLEEP read           oracle         STREAM 0x813e7840
1018295 8009  1     206364723  154 2   SLEEP read           oracle         STREAM 0x95a8f440
1018291 8005  1     206364809  154 3   SLEEP read           oracle         STREAM 0x94d440c0
1018286 8000  1     206364895  154 5   SLEEP read           oracle         STREAM 0x90aaf0c0
1018282 7996  1     206364983  154 3   SLEEP read           oracle         STREAM 0x947b5c00
1018278 7992  1     206365068  154 5   SLEEP read           oracle         STREAM 0x94d72bc0
1018270 7984  1     206365238  154 3   SLEEP read           oracle         STREAM 0x95678880
1018262 7976  1     206365334  154 6   SLEEP read           oracle         STREAM 0x909e5b80
1018253 7967  1     206365424  154 5   SLEEP read           oracle         STREAM 0x8cb5d040
1018237 7951  1     206365768  154 5   SLEEP read           oracle         STREAM 0x95c614c0
1018233 7947  1     206365856  154 0   SLEEP read           oracle         STREAM 0x91143400
1018229 7943  1     206365944  154 3   SLEEP read           oracle         STREAM 0xa53184c0
1018225 7939  1     206366036  154 3   SLEEP read           oracle         STREAM 0x95c084c0
1018221 7935  1     206366120  154 3   SLEEP read           oracle         STREAM 0x7943c4c0
1018217 7931  1     206366202  154 3   SLEEP read           oracle         STREAM 0x79a1f440
1018213 7927  1     206366287  154 6   SLEEP read           oracle         STREAM 0x954390c0
1018209 7923  1     206366369  154 6   SLEEP read           oracle         STREAM 0x84adc440
1018205 7919  1     206366458  154 1   SLEEP read           oracle         STREAM 0x954c8440
1018201 7915  1     206366539  154 3   SLEEP read           oracle         STREAM 0x95857100
1018197 7911  1     206366626  154 3   SLEEP read           oracle         STREAM 0x9084d7c0
1018189 7903  1     206366808  154 0   SLEEP read           oracle         STREAM 0x811027c0
1018175 7887  1     206366998  154 1   SLEEP read           oracle         STREAM 0x811350c0
1018167 7879  1     206367169  154 4   SLEEP read           oracle         STREAM 0x94c710c0
1018163 7875  1     206367252  154 0   SLEEP read           oracle         STREAM 0x9b359040
1018155 7867  1     206367426  154 1   SLEEP read           oracle         STREAM 0x911b8100
1018151 7863  1     206367517  154 2   SLEEP read           oracle         STREAM 0x966df4c0
1018147 7859  1     206367602  154 5   SLEEP read           oracle         STREAM 0x95815bc0
1018139 7851  1     206367770  154 0   SLEEP read           oracle         STREAM 0x954f6c40
1018135 7847  1     206367852  154 4   SLEEP read           oracle         STREAM 0x94cbc480
1018118 7830  1     206368224  154 0   SLEEP read           oracle         STREAM 0x84a46bc0
1018114 7826  1     206368330  154 5   SLEEP read           oracle         STREAM 0x93b69400
1018106 7818  1     206368508  154 6   SLEEP read           oracle         STREAM 0x95209800
1018102 7814  1     206368601  154 3   SLEEP read           oracle         STREAM 0x95afcc00
1018078 7790  1     206368791  154 1   SLEEP read           oracle         STREAM 0x8cdbc040
1018072 7784  1     206368885  154 6   SLEEP read           oracle         STREAM 0x93d17480
1018068 7780  1     206368972  154 6   SLEEP read           oracle         STREAM 0x94f43100
1018056 7768  1     206369254  154 1   SLEEP read           oracle         STREAM 0x8cd7ac40
1018052 7764  1     206369334  154 1   SLEEP read           oracle         STREAM 0x95378c40
1018048 7760  1     206369414  154 4   SLEEP read           oracle         STREAM 0x9110ac00
1018040 7752  1     206369577  154 1   SLEEP read           oracle         STREAM 0x7cc30880
1018036 7748  1     206369658  154 1   SLEEP read           oracle         STREAM 0x9534fbc0
1018032 7744  1     206369760  154 4   SLEEP read           oracle         STREAM 0x79573c00
1018027 7739  1     206369861  154 4   SLEEP read           oracle         STREAM 0x953a5840
1018023 0     0     206369949  154 0   SLEEP read                          STREAM 0x957b8b80
1018019 7731  1     206370043  154 5   SLEEP read           oracle         STREAM 0x90a17c40
1018015 7727  1     206370131  154 3   SLEEP read           oracle         STREAM 0x94d4cc40
1018011 7723  1     206370213  154 2   SLEEP read           oracle         STREAM 0x95e9bb80
1018002 7716  1     206370304  154 6   SLEEP read           oracle         STREAM 0x95afc840
1235    1170  1     269284076  154 1   SLEEP select         fddi4subagt    per_processor_selects+0x40
354882  3583  1     275992361  154 4   SLEEP ksleep         rmiregistry    ksleep(0x79a35ac0)
354794  3583  1     276036799  154 5   SLEEP ksleep         rmiregistry    ksleep(0x8c4fd2c0)
354796  3583  1     276036799  154 5   SLEEP ksleep         rmiregistry    ksleep(0x8c4fd280)
354797  3583  1     276036799  154 5   SLEEP ksleep         rmiregistry    ksleep(0x8c4fd540)
1834    1576  1     317423002  154 0   SLEEP ksleep         prm3d          ksleep(0x798d0400)
1833    1576  1     317657023  154 6   SLEEP ksleep         prm3d          ksleep(0x798d03c0)
4102    3703  1     318331053  154 1   SLEEP select         registrar      per_processor_selects+0x40
1706    1576  1     318341062  154 3   SLEEP ksleep         prm3d          ksleep(0x798d0280)
1850    1576  1     318341062  178 1   ZOMB  lwp_detached_e prm3d          
1828    1576  1     318371348  154 2   SLEEP ksleep         prm3d          ksleep(0x7939b440)
5058    4253  1     318372193  154 4   SLEEP read           mflm_manager   FIFO 0x7914d1c0
5041    4236  1     318372240  154 5   SLEEP ioctl          sfd            sfd_sleeping
1697    1559  1     318382480  154 1   SLEEP ksleep         perflbd        ksleep(0x79d720c0)
1370    1260  1     318383610  154 3   SLEEP ksleep         swci           ksleep(0x798eb280)
1346    1247  1     318383619  154 3   SLEEP ksleep         sdci           ksleep(0x798f8340)
1335    1243  1     318383630  154 0   SLEEP ksleep         hpuxci         ksleep(0x798d0180)
3380    3147  1     318391972  154 1   SLEEP select         cmclconfd      per_processor_selects+0x40
4095    3696  1359  318393660  158 3   SLEEP sigtimedwait   memlogd        pm_sigwait(0x791bf1c0)
4104    3696  1359  318393662  158 6   SLEEP sigwait        memlogd        pm_sigwait(0x792ed040)
4107    3696  1359  318393662  154 6   SLEEP ksleep         memlogd        ksleep(0x79d72880)
3696    3223  3043  318394134  154 3   SLEEP ksleep         caiUxOs        ksleep(0x7939ba00)
1730    1605  1     318398495  154 1   SLEEP select         emsagent       per_processor_selects+0x180
3567    3238  3043  318400813  154 4   SLEEP ksleep         hpaAgent       ksleep(0x79d72640)
3804    3427  1     318401135  154 0   SLEEP ksleep         jre            ksleep(0x7939bbc0)
3552    3223  3043  318401241  154 3   SLEEP ksleep         caiUxOs        ksleep(0x798eb900)
3815    3427  1     318401277  168 1   SLEEP sigwait        jre            pm_sigwait(0x78fb91c0)
3811    3427  1     318401278  154 1   SLEEP ksleep         jre            ksleep(0x7939bb80)
3240    3043  1     318401378  154 6   SLEEP ksleep         awservices     ksleep(0x798f8600)
3137    2954  1     318401555  154 6   STOP  ksleep         vxsvc          ksleep(0x798f8440)
3571    3043  1     318401782  154 3   SLEEP recv           awservices     STREAM 0x88079840
3556    3043  1     318401782  154 0   SLEEP recv           awservices     STREAM 0x840d87c0
3582    3043  1     318401782  154 4   SLEEP recv           awservices     STREAM 0x805b7440
3513    3043  1     318401782  154 2   SLEEP recv           awservices     STREAM 0x840c5880
3528    3213  3043  318401856  154 6   SLEEP ksleep         caiLogA2       ksleep(0x798d0a80)
3603    3245  3043  318401869  154 0   SLEEP ksleep         proAgent       ksleep(0x798d0f80)
3577    3244  3043  318401890  154 5   SLEEP ksleep         prfAgent       ksleep(0x798d0e00)
3303    3074  3043  318401914  154 4   SLEEP accept         aws_orb        SOCKET 0x8006ea00
3532    3043  1     318401984  154 0   SLEEP recv           awservices     STREAM 0x840d8040
3602    3245  3043  318402085  154 4   SLEEP recv           proAgent       STREAM 0x806ba080
3601    3043  1     318402085  154 4   SLEEP recv           awservices     STREAM 0x880e97c0
3579    3245  3043  318402086  154 3   SLEEP ksleep         proAgent       ksleep(0x798d0e80)
3569    3244  3043  318402094  154 3   SLEEP ksleep         prfAgent       ksleep(0x79d726c0)
3581    3043  1     318402098  154 4   SLEEP ksleep         awservices     ksleep(0x798eba80)
3575    3043  1     318402098  154 4   SLEEP recv           awservices     STREAM 0x7bff5480
3576    3244  3043  318402098  154 6   SLEEP recv           prfAgent       STREAM 0x841ae7c0
3578    3043  1     318402098  158 4   SLEEP waitpid        awservices     wait1(0x84022040)
3574    3244  3043  318402099  154 2   SLEEP ksleep         prfAgent       ksleep(0x798d0d80)
3570    3043  1     318402103  154 6   SLEEP ksleep         awservices     ksleep(0x798f8bc0)
3568    3043  1     318402103  158 2   SLEEP waitpid        awservices     wait1(0x84022040)
3554    3238  3043  318402104  154 6   SLEEP ksleep         hpaAgent       ksleep(0x798eb980)
3566    3238  3043  318402104  154 0   SLEEP recv           hpaAgent       STREAM 0x805b7080
3565    3043  1     318402104  154 6   SLEEP recv           awservices     STREAM 0x7bff5840
3564    3238  3043  318402105  154 0   SLEEP ksleep         hpaAgent       ksleep(0x798eba00)
3550    3043  1     318402130  154 2   SLEEP recv           awservices     STREAM 0x7c1fb400
3551    3223  3043  318402130  154 0   SLEEP recv           caiUxOs        STREAM 0x880f6c40
3553    3043  1     318402130  158 5   SLEEP waitpid        awservices     wait1(0x84022040)
3530    3223  3043  318402130  154 1   SLEEP ksleep         caiUxOs        ksleep(0x798d0bc0)
3555    3043  1     318402130  154 4   SLEEP ksleep         awservices     ksleep(0x798d0cc0)
3531    3043  1     318402177  154 6   SLEEP ksleep         awservices     ksleep(0x798d0b00)
3529    3043  1     318402177  158 1   SLEEP waitpid        awservices     wait1(0x84022040)
3526    3043  1     318402177  154 4   SLEEP recv           awservices     STREAM 0x880f6880
3511    3213  3043  318402177  154 5   SLEEP ksleep         caiLogA2       ksleep(0x798d0980)
3527    3213  3043  318402177  154 2   SLEEP recv           caiLogA2       STREAM 0x841ae040
1334    1243  1     318402182  154 2   SLEEP ksleep         hpuxci         ksleep(0x7939b280)
3510    3043  1     318402260  158 1   SLEEP waitpid        awservices     wait1(0x84022040)
3512    3043  1     318402260  154 1   SLEEP ksleep         awservices     ksleep(0x79a35840)
3232    3044  1     318402382  158 0   SLEEP waitpid        cmclconfd      wait1(0x84027040)
3384    3043  1     318402462  154 0   SLEEP recv           awservices     STREAM 0x7bfc4bc0
3419    3043  1     318402664  154 3   SLEEP recv           awservices     STREAM 0x7c0c1c00
3382    3148  3043  318402689  154 1   SLEEP ksleep         aws_sadmin     ksleep(0x798eb700)
3415    3148  3043  318402689  154 5   SLEEP ksleep         aws_sadmin     ksleep(0x798eb800)
3471    3148  3043  318402691  154 1   SLEEP ksleep         aws_sadmin     ksleep(0x798f8a80)
3446    3168  3043  318402857  154 1   SLEEP ksleep         aws_snmp       ksleep(0x79a35780)
3442    3168  3043  318402857  154 3   SLEEP ksleep         aws_snmp       ksleep(0x79a35680)
3417    3168  3043  318402857  154 4   SLEEP ksleep         aws_snmp       ksleep(0x79d722c0)
3444    3168  3043  318402857  154 4   SLEEP ksleep         aws_snmp       ksleep(0x79a35700)
3445    3168  3043  318402857  154 6   SLEEP poll           aws_snmp       per_processor_selects+0x180
3443    3168  3043  318402857  154 5   SLEEP poll           aws_snmp       per_processor_selects+0x140
3425    3168  3043  318402857  154 6   SLEEP ksleep         aws_snmp       ksleep(0x79d723c0)
3440    3168  3043  318402858  154 1   SLEEP ksleep         aws_snmp       ksleep(0x79a35600)
3438    3168  3043  318402858  154 1   SLEEP ksleep         aws_snmp       ksleep(0x79a35580)
3441    3168  3043  318402858  154 1   SLEEP poll           aws_snmp       per_processor_selects+0x40
3439    3168  3043  318402858  154 1   SLEEP poll           aws_snmp       per_processor_selects+0x40
3436    3168  3043  318402862  154 4   SLEEP ksleep         aws_snmp       ksleep(0x798d0900)
3423    3043  1     318402866  154 0   SLEEP recv           awservices     STREAM 0x7c0c1840
3424    3168  3043  318402866  154 5   SLEEP recv           aws_snmp       STREAM 0x8406cc40
3422    3168  3043  318402867  154 3   SLEEP ksleep         aws_snmp       ksleep(0x79d72340)
3413    3043  1     318402871  154 6   SLEEP recv           awservices     STREAM 0x80078080
3414    3148  3043  318402871  154 3   SLEEP recv           aws_sadmin     STREAM 0x880f64c0
3416    3043  1     318402871  158 6   SLEEP waitpid        awservices     wait1(0x84022040)
3418    3043  1     318402871  154 1   SLEEP ksleep         awservices     ksleep(0x79a35400)
3408    3148  3043  318402873  154 5   SLEEP ksleep         aws_sadmin     LVM vg05/lv128
3383    3043  1     318402900  154 5   SLEEP ksleep         awservices     ksleep(0x79d72200)
3381    3043  1     318402900  158 0   SLEEP waitpid        awservices     wait1(0x84022040)
3283    3080  3057  318402923  154 1   SLEEP select         cmtaped        per_processor_selects+0x40
3346    3131  1559  318402999  154 0   SLEEP ksleep         agdbserver     ksleep(0x79a35280)
1678    1559  1     318402999  154 5   SLEEP select         perflbd        per_processor_selects+0x140
3348    1559  1     318403031  154 4   SLEEP select         perflbd        per_processor_selects+0x100
3112    2931  1559  318403034  154 3   SLEEP ksleep         rep_server     ksleep(0x798d06c0)
3339    2954  1     318403056  154 5   STOP  ksleep         vxsvc          ksleep(0x7939b500)
3221    2954  1     318403056  154 6   STOP  ksleep         vxsvc          ksleep(0x79d72100)
3338    2954  1     318403058  152 6   ZOMB  lwp_detached_e vxsvc          
3271    3043  1     318403102  154 6   SLEEP recv           awservices     STREAM 0x7c0414c0
3280    3074  3043  318403183  154 4   SLEEP ksleep         aws_orb        ksleep(0x79ac1140)
3269    3074  3043  318403183  154 1   SLEEP ksleep         aws_orb        ksleep(0x798f8680)
3311    3074  3043  318403183  154 6   SLEEP accept         aws_orb        STREAM 0x8406c4c0
3304    3074  3043  318403188  154 2   SLEEP accept         aws_orb        STREAM 0x80078bc0
3281    3078  1     318403190  158 4   SLEEP waitpid        dtrc           wait1(0x7c030040)
3279    3074  3043  318403203  154 3   SLEEP recv           aws_orb        STREAM 0x80073400
3278    3043  1     318403203  154 0   SLEEP recv           awservices     STREAM 0x7c050100
3277    3074  3043  318403208  154 0   SLEEP ksleep         aws_orb        ksleep(0x798f8700)
3268    3043  1     318403214  158 5   SLEEP waitpid        awservices     wait1(0x84022040)
3270    3043  1     318403214  154 0   SLEEP ksleep         awservices     ksleep(0x798eb580)
3231    3043  1     318403416  154 5   SLEEP ksleep         awservices     ksleep(0x798f8500)
3236    3043  1     318403430  154 3   SLEEP ksleep         awservices     ksleep(0x798f8540)
3237    3043  1     318403430  154 3   SLEEP ksleep         awservices     ksleep(0x798f8580)
3176    2989  1     318403675  154 1   SLEEP nfssvc         nfsd           svc_run(0x4044d140)
3193    3006  2993  318403675  154 3   SLEEP nfssvc         nfsd           svc_run(0x4044d1c0)
3196    3009  2989  318403686  154 1   SLEEP nfssvc         nfsd           svc_run(0x4044d140)
3195    3008  2994  318403686  154 5   SLEEP nfssvc         nfsd           svc_run(0x4044d180)
3182    2995  2989  318403686  154 1   SLEEP nfssvc         nfsd           svc_run(0x4044d0c0)
3190    3003  2992  318403686  154 2   SLEEP nfssvc         nfsd           svc_run(0x4044d080)
3197    3010  2993  318403686  154 4   SLEEP nfssvc         nfsd           svc_run(0x4044d1c0)
3179    2992  2989  318403686  154 4   SLEEP nfssvc         nfsd           svc_run(0x4044d080)
3198    3011  2994  318403686  154 0   SLEEP nfssvc         nfsd           svc_run(0x4044d180)
3192    3005  2991  318403686  154 3   SLEEP nfssvc         nfsd           svc_run(0x4044d100)
3178    2991  2989  318403686  154 3   SLEEP nfssvc         nfsd           svc_run(0x4044d100)
3174    2987  1     318403690  154 6   SLEEP poll           nfsd           per_processor_selects+0x180
3177    2990  2989  318403690  154 0   SLEEP nfssvc         nfsd           svc_run(0x4044d040)
3183    2996  2990  318403690  154 1   SLEEP nfssvc         nfsd           svc_run(0x4044d040)
3185    2998  2990  318403690  154 6   SLEEP nfssvc         nfsd           svc_run(0x4044d040)
3146    2954  1     318403714  154 4   STOP  poll           vxsvc          per_processor_selects+0x100
3149    2954  1     318403723  154 5   STOP  ksleep         vxsvc          ksleep(0x798d0580)
1306    1234  1     318404069  154 4   SLEEP select         dmisp          per_processor_selects+0x100
1330    1243  1     318404069  154 1   SLEEP lwp_wait       hpuxci         thread_wait(0x794a4040)
3108    1234  1     318404094  137 5   ZOMB  lwp_detached_e dmisp          
3109    1243  1     318404094  154 5   SLEEP recv           hpuxci         sosleep(0x7909e940)
1693    1559  1     318412184  154 4   SLEEP ksleep         perflbd        ksleep(0x798d0240)
1336    1243  1     318412632  154 6   SLEEP ksleep         hpuxci         ksleep(0x798f8300)
1347    1247  1     318412632  154 1   SLEEP ksleep         sdci           ksleep(0x798eb1c0)
1371    1260  1     318412632  154 3   SLEEP ksleep         swci           ksleep(0x7939b340)
1700    1576  1     318413044  154 6   SLEEP ksleep         prm3d          ksleep(0x798d02c0)
1842    1576  1     318413066  154 3   SLEEP accept         prm3d          STREAM 0x7bc7f040
1841    1576  1     318413076  154 3   SLEEP accept         prm3d          STREAM 0x7bc750c0
1843    1576  1     318413082  154 4   SLEEP ksleep         prm3d          ksleep(0x79a35180)
1344    1247  1     318413121  154 2   SLEEP ksleep         sdci           ksleep(0x798eb180)
1368    1260  1     318413121  154 1   SLEEP ksleep         swci           ksleep(0x7939b300)
1832    1576  1     318413168  154 0   SLEEP ksleep         prm3d          ksleep(0x798d0380)
1831    1576  1     318413168  154 5   SLEEP ksleep         prm3d          ksleep(0x798d0340)
1528    1411  1     318413316  154 1   SLEEP accept         acdistagn      STREAM 0x79486040
1617    1500  1     318413364  154 2   SLEEP poll           ttd            per_processor_selects+0x80
1679    1559  1     318413485  -32 4   SLEEP sigwait        perflbd        pm_sigwait(0x79471040)
1479    1362  1     318413861  154 1   SLEEP read           envd           FIFO 0x78f88640
1340    1247  1     318414143  154 2   SLEEP lwp_wait       sdci           thread_wait(0x79a36040)
1359    1260  1     318414343  154 5   SLEEP lwp_wait       swci           thread_wait(0x79ac2040)
1448    1260  1     318414449  154 0   SLEEP recv           swci           sosleep(0x793be1c0)
1422    1247  1     318414520  168 1   SLEEP sigwait        sdci           pm_sigwait(0x78fb90c0)
1421    1247  1     318414527  154 1   SLEEP recv           sdci           sosleep(0x78e3b7c0)
1317    1234  1     318414572  154 6   SLEEP ksleep         dmisp          ksleep(0x798f8180)
1360    1260  1     318414601  168 1   SLEEP sigwait        swci           pm_sigwait(0x78fb9080)
1331    1243  1     318414620  168 3   SLEEP sigwait        hpuxci         pm_sigwait(0x791bf0c0)
1319    1234  1     318415109  154 2   SLEEP ksleep         dmisp          ksleep(0x798f8200)
1321    1234  1     318415109  154 0   SLEEP ksleep         dmisp          ksleep(0x798f8280)
1318    1234  1     318415109  154 3   SLEEP ksleep         dmisp          ksleep(0x798f81c0)
1320    1234  1     318415109  154 3   SLEEP ksleep         dmisp          ksleep(0x798f8240)
1316    1234  1     318415109  154 5   SLEEP ksleep         dmisp          ksleep(0x798f8140)
1322    1234  1     318415110  154 3   SLEEP ksleep         dmisp          ksleep(0x798f82c0)
1308    1234  1     318415110  154 5   SLEEP ksleep         dmisp          ksleep(0x7939b200)
1314    1234  1     318415110  154 4   SLEEP ksleep         dmisp          ksleep(0x798f80c0)
1315    1234  1     318415110  154 3   SLEEP ksleep         dmisp          ksleep(0x798f8100)
1304    1234  1     318415126  154 3   SLEEP ksleep         dmisp          ksleep(0x7939b100)
1307    1234  1     318415126  154 3   SLEEP ksleep         dmisp          ksleep(0x7939b1c0)
1299    1234  1     318415126  154 0   SLEEP lwp_wait       dmisp          thread_wait(0x798e6040)
1302    1234  1     318415135  154 2   SLEEP ksleep         dmisp          ksleep(0x7939b080)
1300    1234  1     318415138  168 3   SLEEP sigwait        dmisp          pm_sigwait(0x791bf080)
867     804   1     318416245  154 4   SLEEP poll           rpc.lockd      per_processor_selects+0x100
1204    1139  1     318416287  154 3   SLEEP select         trapdestagt    per_processor_selects+0xc0
818     755   0     318421380  152 0   SLEEP n/a            nfskd          nfs_srvr_debug+0x10
706     643   1     318424476  127 3   SLEEP semop          ntl_reader     sema[2][1]+0x4
694     633   1     318424706  127 0   SLEEP read           nktl_daemon    netdiag_ques+0x44
672     611   1     318424744  155 3   SLEEP msgrcv         ptydaemon      msgque[0]+0x5c
599     14    0     318425004  148 0   SLEEP n/a            ioconfigd      ve_thread_to_terminate
44      44    0     318426433  100 0   SLEEP n/a            sblksched      streams_blk_sync
43      43    0     318426433  100 0   SLEEP n/a            sblksched      streams_blk_sync
9       9     0     318426546  100 0   SLEEP n/a            strmem         __preinit_end+0x978
10      10    0     318426546  100 0   SLEEP n/a            strweld        weldq_runq
8       8     0     318426546  100 0   SLEEP n/a            supsched       streams_up_runq
11      11    0     318426546  100 0   SLEEP n/a            strfreebd      str_freeb_idle

			==================
			= Kernel Patches =
			==================

PHKL_22857	PHKL_22874	PHKL_22994	PHKL_23196	PHKL_23203
PHKL_23225	PHKL_23239	PHKL_23242	PHKL_23246	PHKL_23250
PHKL_23335	PHKL_23337	PHKL_23400	PHKL_23401	PHKL_23423
PHKL_23445	PHKL_23505	PHKL_23609	PHKL_23626	PHKL_23642
PHKL_23811	PHKL_23934	PHKL_23957	PHKL_23995	PHKL_23999
PHKL_24026	PHKL_24037	PHKL_24046	PHKL_24047	PHKL_24056
PHKL_24163	PHKL_24188	PHKL_24253	PHKL_24278	PHKL_24343
PHKL_24408	PHKL_24507	PHKL_24548	PHKL_24551	PHKL_24552
PHKL_24554	PHKL_24555	PHKL_24556	PHKL_24558	PHKL_24562
PHKL_24563	PHKL_24565	PHKL_24568	PHKL_24569	PHKL_24571
PHKL_24574	PHKL_24575	PHKL_24577	PHKL_24578	PHKL_24579
PHKL_24580	PHKL_24581	PHKL_24582	PHKL_24583	PHKL_24585
PHKL_24652	PHKL_24677	PHKL_24730	PHKL_24751	PHKL_24779
PHKL_24960	PHKL_25022	PHKL_25080	PHKL_25123	PHKL_25165
PHKL_25166	PHKL_25209	PHKL_25212	PHKL_25233	PHKL_25238
PHKL_25248	PHKL_25304	PHKL_25351	PHKL_25362	PHKL_25367
PHKL_25368	PHKL_25375	PHKL_25493	PHKL_25552	PHKL_25593
PHKL_25602	PHKL_25610	PHKL_25614	PHKL_25652	PHKL_25728
PHKL_25729	PHKL_25761	PHKL_25773	PHKL_25778	PHKL_25869
PHKL_25871	PHKL_25885	PHKL_25886	PHKL_26002	PHKL_26032
PHKL_26037	PHKL_26074	PHKL_26104	PHKL_26201	PHKL_26230
PHKL_26241	PHKL_26386	PHKL_26407	PHKL_26425	PHKL_26467
PHKL_26519	PHKL_26552	PHKL_26705	PHKL_26734	PHKL_26743
PHKL_26755	PHKL_26761	PHKL_26833	PHKL_26847	PHKL_26893
PHKL_26938	PHKL_26979	PHKL_27025	PHKL_27054	PHKL_27091
PHKL_27092	PHKL_27094	PHKL_27096	PHKL_27121	PHKL_27152
PHKL_27153	PHKL_27154	PHKL_27155	PHKL_27156	PHKL_27172
PHKL_27179	PHKL_27219	PHKL_27266	PHKL_27278	PHKL_27283
PHKL_27294	PHKL_27321	PHKL_27498	PHKL_27531	PHKL_27532
PHKL_27669	PHKL_27682	PHKL_27688	PHKL_27714	PHKL_27715
PHKL_27757	PHKL_27992	PHKL_28410	PHNE_22878	PHNE_23465
PHNE_23502	PHNE_23594	PHNE_23932	PHNE_24035	PHNE_24131
PHNE_24211	PHNE_24473	PHNE_24492	PHNE_24506	PHNE_24910
PHNE_25083	PHNE_25388	PHNE_25627	PHNE_25644	PHNE_26326
PHNE_26710	PHNE_26728	PHNE_26939	PHNE_27218	

```

