# System panic with kalloc: out of kernel virtual space.

## System panic with kalloc: out of kernel virtual space.

```
            System crash dump analysis report
            =================================

Symptom
-------
 System behaved as panic. Panic string was showing as 
 kalloc: out of kernel virtual space.

Root Cause
----------
 The sysmap_32bit is completely depleted. There is not a single
 page available. It's not the kindest way of telling outside
 world that the box is about to panic due to empty kernel memory map.
 The major contributer comes from the usage of 512 bytes memory bucket.

Action Plan
-----------
 The following patches are recommended.

  Catalog     Text      (21 patches 57MB)
  ----------  ---------------------------------------------
  PHCO_23651  fsck_vxfs(1M) cumulative patch
  PHCO_24437  LVM commands cumulative patch
  PHCO_25902  cumulative SAM/ObAM patch
  PHCO_27370  mksf(1M) cumulative patch
  PHCO_29027  libsec cumulative patch
  PHCO_29380  user/group(add/mod/del)(1M) cumulative patch
  PHKL_18543  PM/VM/UFS/async/scsi/io/DMAPI/JFS/perf patch
  PHKL_20016  2nd CPU not recognized in G70/H70/I70
  PHKL_23409  NFS, Large Data Space, kernel memory leak
  PHKL_24187  ioscan performance gain for SCSI Subsystem
  PHKL_28150  LVM Cumulative Patch w/Performance Upgrades
  PHKL_29385  IDS/9000; syscalls; eventports; dup2() race
  PHKL_29434  POSIX AIO;getdirentries;MVFS;rcp;mmap/IDS;
  PHKL_29693  VxFS 3.1 cumulative patch: CR_EIEM
  PHKL_29834  SCSI IO Subsystem Cumulative Patch
  PHKL_30190  Probe,IDDS,PM,VM,PA-8700,AIO,T600,FS,PDC,CLK
  PHNE_26096  telnet kernel and telnetd(1M) patch
  PHNE_27821  Streams Pty cumulative patch
  PHNE_27902  Cumulative STREAMS Patch
  PHNE_29473  cumulative ARPA Transport patch
  PHNE_29530  LAN product cumulative patch
  
Detail Analysis
---------------

Dump time Mon Jun 28 11:35:46 2004 UTC-8
System has been up 122 days, 14 hours, 56 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/K360
HP-UX version    : B.11.00 (32-bit Kernel)
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : PCXU (180 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 2.37  1.32  1.29  

			================
			= Crash Events =
			================

Panic string : kalloc: out of kernel virtual space

Stack Traces for all other Crash events 
=======================================

==============  EVENT  ============================
= Event #1 is PANIC on CPU #0
= p crash_event_t 0x21020
= p rpb_t 0x6f6e08
= Using pc from pim.wide.rp_rp_hi = 0x26b044
==============  EVENT  ============================
SR5=0x076db400
        SP         RP Return Name
0x7fff13a8 0x0026b044 panic+0x14
0x7fff1368 0x0033dffc report_trap_or_int_and_panic+0x4c
0x7fff1328 0x000de1ac trap+0x514
0x7fff11f8 0x0027f40c thandler+0xbdc
+-------------  TRAP  ----------------------------
|  Trap type 18 in KERNEL mode at 0x378e0 (pgcopy+0x1e0)
|  p struct save_state 0x76db400.0x7fff0d48
+-------------  TRAP  ----------------------------
SR5=0x076db400
        SP         RP Return Name
0x7fff0d48 0x000378e0 pgcopy+0x1e0
0x7fff0d48 0x000e4d8c hdl_cwfault+0x2f0
0x7fff0c20 0x000db694 virtual_fault+0xd8c
0x7fff0b20 0x000df77c vfault+0xf4
0x7fff0ad0 0x000ddfb4 trap+0x31c
0x7fff09a0 0x0027f40c thandler+0xbdc
+-------------  TRAP  ----------------------------
|  Trap type 15 in USER mode at 0x5bd6400.0x7f7dcba3 (???)
|  p struct save_state 0x76db400.0x7fff04f0
+-------------  TRAP  ----------------------------


			==================
			= Message Buffer =
			==================

panic: kalloc: out of kernel virtual space 
PC-Offset Stack Trace (read across, top of stack is 1st): 
  0x0026b07c  0x00129474  0x000db994  0x000ea3e4  0x00146874  0x00152eb0 
  0x00090c70  0x00114404  0x00035e80
End Of Stack 
Trap Type 18 (Data memory protection fault): 
  Instruction Address (pcsq.pcoq) = 0x0.0x378e0 
  Instruction (iir) = 0x0f20909c (load/store) 
  Target Address (isr.ior) = 0x0.0xffffffff 
  gr25 = 0xffffffff 
  gr00 = 0x00000000 
  gr28 = 0x00038740 
  Savestate Ptr (ssp) = 0x76db400.0x7fff0d48 
  Savestate Return Pointer (ss_rp) = 0xe4d8c 
 
			==================
			= Memory Globals =
			==================

Physical Memory     = 327680 pages (1.25 GB)
Free Memory         = 4438 pages (17.34 MB)
Average Free Memory = 3019 pages (11.79 MB)
gpgslim             = 2456 pages (9.59 MB)
lotsfree            = 8192 pages (32.00 MB)
desfree             = 1024 pages (4.00 MB)
minfree             = 256 pages (1.00 MB)

Note:  There are 70 deactivated processes !

deactload: 42.20  12.48  4.48  
maxpendpageouts = 1307
pageoutrate     = 1307  curr_pgrate = 4515
min_pgrate      = 25    max_pgrate  = 3827
lowmemdeact     = 0     thrashdeact = 1491


			=======================
			= Kernel Memory Usage =
			=======================
----------------------------------------------------------------------
Physical memory usage summary (in page/byte/percent):

Physmem             =   327680    1.2g 100%  Physical memory
  Freemem           =     4438   17.3m   1%  Free physical memory
  Used              =   323242    1.2g  99%  Used physical memory
    System          =   278917    1.1g  85%  By kernel:
      text          =     1684    6.6m   1%   text
      data          =      136  544.0k   0%   data
      bss           =      603    2.4m   0%   bss
      Static        =    16701   65.2m   5%   for text/static data
      Dynamic       =   245173  957.7m  75%   for dynamic data
      Bufcache      =    16383   64.0m   5%   for buffer cache
      Eqmem         =       20   80.0k   0%   for equiv. mapped memory
      SCmem         =      640    2.5m   0%   for critical memory
    User            =    44317  173.1m  14%  By user processes:
      Uarea         =     1100    4.3m   0%   for thread uareas
    Disowned        =        8   32.0k   0%  Disowned pages

----------------------------------------------------------------------
Kernel dynamic memory usage (in page/byte/percent):

Dynamic             =   245173  957.7m  75%  Kernel dynamic memory
  MALLOC            =   227043  886.9m  69%  Memory buckets
    bucket[5]       =     1280    5.0m   0%   size    32 bytes
    bucket[6]       =      244  976.0k   0%   size    64 bytes
    bucket[7]       =     2501    9.8m   1%   size   128 bytes
    bucket[8]       =     1867    7.3m   1%   size   256 bytes
    bucket[9]       =   213742  834.9m  65%   size   512 bytes
    ^^^^^^^^^^^^^^^^^^^^^^^^^^  // ---- how come so large ???

    bucket[10]      =     2943   11.5m   1%   size  1024 bytes
    bucket[11]      =     1123    4.4m   0%   size  2048 bytes
    bucket[12]      =      137  548.0k   0%   size  4096 bytes
    bucket[13]      =      240  960.0k   0%   size     2 pages
    bucket[14]      =       30  120.0k   0%   size     3 pages
    bucket[15]      =        8   32.0k   0%   size     4 pages
    bucket[16]      =        5   20.0k   0%   size     5 pages
    bucket[17]      =       36  144.0k   0%   size     6 pages
    bucket[18]      =        0    0.0k   0%   size     7 pages
    bucket[19]      =      208  832.0k   0%   size     8 pages
    bucket[20]      =     2679   10.5m   1%   size >   8 pages
  Reserved          =       13   52.0k   0%  Reserved pools
  Kalloc            =    17780   69.5m   5%  kalloc()
    SuperPagePool   =        0    0.0k   0%    Kernel superpage cache
    BufcacheBufs    =    15994   62.5m   5%    Buffer cache bufs
    BufcacheHash    =      640    2.5m   0%    Buffer cache hash heads
    Other           =     1146    4.5m   0%    Other...
  Eqalloc           =      337    1.3m   0%  eqalloc()

			=====================
			= User Memory Usage =
			=====================


Top 10 Processes sorted by physical size (in pages):

pid   command        virtual  physical
----- -------------- -------- --------
2100  cmcld          2290     2100    
3855  oracle         67570    918     
3866  oracle         67565    913     
3915  oracle         67565    907     
3885  oracle         67565    907     
3794  oracle         67565    879     
25906 oracle         67673    861     
3928  oracle         67179    856     
25878 oracle         67633    834     
3265  oracle         67609    796     

			========================
			= Buffer Cache Globals =
			========================

dbc_max_pct           = 50 %
dbc_min_pct           = 5 %
dbc current pct       = 5.0 %
bufpages              = 16383 pages (64.00 MB)
Number of buf headers = 159940

fixed_size_cache = 0 
dbc_parolemem    = 0 
dbc_stealavg     = 16383 
dbc_ceiling      = 163840 pages (640.00 MB)
dbc_nbuf         = 8192 
dbc_bufpages     = 16384 pages (64.00 MB)
dbc_vhandcredit  = 1026141 
orignbuf         = 0 
origbufpages     = 0 pages

			====================
			= Swap Information =
			====================

swapinfo -mt emulation
======================

             Mb      Mb      Mb   PCT  START/      Mb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev        1900     531    1369   28%       0       -    1  LVM vg00/lv2
reserve       -     414    -414
total      1900     945     955   50%       -       0    -


			==========
			= Sysmap =
			==========

p4>p4_sysmap -a
sysmap_32bit has a configured mapsize of 23328 entries
   index             m_addr           m_size
       0           0x9d6900     sysmap_32bit
       1                  0                0

Statistics :
Number of entries used = 1
Total free             = 0 pages
Largest free size      = 0 pages
Average free size      = 0.00 pages

Fragmentation :
sysmap_32bit has a configured mapsize of 23328 entries
   index             m_addr           m_size
       0           0x9d6900     sysmap_32bit
       1                  0                0
q4> run PrintMap sysmap
Loading the map entries, this may take a while...
The sysmap_32bit is completely depleted. 
There is not a single page available!

The sysmap is sized for a maximum of 23328 entries

```

