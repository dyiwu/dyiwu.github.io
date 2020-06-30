# System hang due to NFS server down and out of swap space.

## System hang due to NFS server down and out of swap space.

```

System crash dump analysis report
=================================
Symptom
-------
  System behaved as hang, TOC dump be taken for root cause
  finding.

Root Cause
----------
  NFS server down which leads process sleeping in nfs3_sleep(),
  and the system swap space resource is exhausted. 

Action Plan
-----------

  1. Add more swap space
  2. Size down kernel parameter dbc_max_pct from 50 to 20,
     to free more memory for application process usage.

Detail Analysis
---------------
- Why system behaved as hang,

  Look at the wait channels, we do have 63 process are waiton
  nfs3_lookup()

Most Common Wait Channels
=========================
	                             ticks since run:
Wait Channel                  count  longest    shortest
------------                  -----  ---------- ----------
*uptr+0                       78     947806283  85
nfs3_lookup(0x2c03800)        63     3255950    1394889
vx_inactive_thread_sv         50     99520      1103
selwait                       37     1095467310 44
lvmkd_q                       6      1905       1905
svc_run(0x2043ea0)            4      1095460218 1095460186
async_bufhead                 4      3326564    3326564
ticks_since_boot              2      0          0

The mount points are:
p4> Mount
Dev_t      Devicefile                 Mount point              Type struct vfs
0x40000003 /dev/root                  /                        vxfs 0x25a1000
0x40000001 /dev/vg00/lvol1            /stand                    hfs 0x280b000
0x40000006 /dev/vg00/lvol6            /var                     vxfs 0x2717800
0x40000005 /dev/vg00/lvol5            /tmp                     vxfs 0x294a000
0x40000004 /dev/vg00/lvol4            /opt                     vxfs 0x2972800
0x40030001 /dev/vgpm/lvol1            /d/pm                    vxfs 0x2982000
0x40000008 /dev/vg00/nmsvar           /d/nmsvar                vxfs 0x2992800
0x40000007 /dev/vg00/nmsopt           /d/nmsopt                vxfs 0x29c0000
0x40000009 /dev/vg00/nmsetc           /d/nmsetc                vxfs 0x29d0800
0x40000010 /dev/vg00/mail             /d/mail                  vxfs 0x29e2000
0x40000011 /dev/vg00/home             /d/home                  vxfs 0x29f2800
0x40020001 /dev/vgdb01/db01           /d/db01                  vxfs 0x2a02000
0x40000012 /dev/vg00/bufmd            /d/bufmd                 vxfs 0x2a50000
0xff000000 tpnmsmd1:(pid834)          /net                      nfs 0x223d800
0xff000001 nmsdb:/d/bufds/pm/meas     /d/pm/meas               nfs3 0x223d000
0xff000002 nmscom:/d/nmsetc           /m/nmsetc                nfs3 0x223e000

What the nfsd are doing :

p4> grep nfsd crashinfo.txt
1731    1674  1     1095460186 154 0   SLEEP nfssvc  nfsd svc_run(0x2043ea0)
1739    1682  1674  1095460211 154 0   SLEEP nfssvc  nfsd svc_run(0x2043ea0)
1738    1681  1674  1095460212 154 0   SLEEP nfssvc  nfsd svc_run(0x2043ea0)
1736    1679  1674  1095460218 154 0   SLEEP nfssvc  nfsd svc_run(0x2043ea0)

What process are stuck in nfs :
p4> grep nfs3_lookup crashinfo.txt
2291    11057 11052 1394889    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
2279    11045 11036 1400946    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
2220    10986 10982 1455896    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
2208    10974 10970 1461892    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1923    10699 10695 1515875    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1911    10687 10683 1521867    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1836    10612 10608 1575942    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1824    10600 10596 1581939    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1528    10314 10310 1635896    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1517    10303 10299 1641895    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1473    10259 10255 1695900    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1460    10247 10243 1701892    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1167    9960  9956  1755926    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1153    9946  9939  1761890    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1109    9902  9898  1815897    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
1097    9890  9886  1821896    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
816     9611  9607  1875915    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
804     9599  9595  1881906    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
726     9517  9513  1935892    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
714     9504  9500  1941887    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
431     9225  9221  1995946    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
419     9213  9209  2001944    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
371     9165  9161  2055874    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
358     9152  9148  2061866    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29972   8866  8862  2115923    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29957   8851  8845  2121888    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29918   8812  8808  2175868    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29905   8799  8795  2181878    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29624   8519  8515  2235883    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29611   8506  8502  2241877    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29538   8433  8429  2295956    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29525   8420  8416  2301939    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29243   8137  8133  2355884    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29230   8124  8120  2361880    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29182   8076  8072  2415919    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
29169   8063  8059  2421916    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28884   7775  7771  2475940    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28870   7761  7754  2481899    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28827   7718  7714  2535892    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28814   7705  7701  2541894    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28534   7425  7421  2595899    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28521   7412  7408  2601893    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28448   7339  7335  2655887    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28435   7326  7322  2661882    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28153   7044  7040  2715957    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28140   7031  7027  2721955    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28100   6991  6987  2775941    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
28086   6977  6973  2781938    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27803   6694  6690  2835951    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27790   6681  6674  2841910    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27715   6606  6602  2895943    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27702   6593  6589  2901937    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27431   6322  6318  2955911    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27417   6308  6304  2961901    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27344   6235  6231  3015891    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27331   6222  6218  3021888    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27053   5945  5941  3075915    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
27039   5931  5927  3081912    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
26998   5890  5886  3135906    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
26985   5877  5873  3141903    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
26700   5592  5588  3195906    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
26687   5579  5572  3201867    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)
26646   5538  5534  3255950    138 0   SLEEP stat csh  nfs3_lookup(0x2c03800)

All are csh process, so interactive processes hang over there.

Trace the csh process to see where is hanging,
p4> trace -W pid 5538
proc[90] pid=5538  cmd="csh -c rm /d/pm/meas/wd2bss/ERR*"
Process  : p proc_t    0x14305c0
           proc[90] pid=5538 csh
Kthread  : p kthread_t 0x14dec40
Using PCB: p user_t 0x51e8.0x7fff0000
SR5=0x51e8
        SP  FUNC   ( ARG0, ARG1, ARG2, ARG3 )
0x7fff0fe0  _sleep+0x4a8   ( 0x2c03800, 0x28a, n/a, n/a )
0x7fff0ee0  nfs3_lookup+0x7c   ( n/a, 0x7fff0c60, 0x7fff0c58, n/a )
0x7fff0e80  locallookuppn+0xdc   ( 0x7fff0b40, 0x1, 0x0, 0x7fff0a78 )
0x7fff0c00  lookuppn+0xf4   ( 0x7fff0b40, 0x1, 0x0, 0x7fff0a78 )
0x7fff0b80  lookupname+0x34   ( 0x7f7e05c8, 0x0, 0x1, 0x0 )
0x7fff0b30  stat1+0x3c   ( 0x7fff0258, 0x1, 0x7fff0000, n/a )
0x7fff0a68  stat+0x1c   ( 0x7fff0258, n/a, n/a, 0x1288e4 )
0x7fff0a38  syscall+0x1c8   ( n/a, n/a, n/a, n/a )
0x7fff09a0  syscallinit+0x5b0   ( n/a, n/a, n/a, n/a )

p4> p struct pathname 0x51e8.0x7fff0b40
0x51e8.0x7fff0b40
0x7fff0b40 :
0x7fff0b40 struct pathname {
0x7fff0b40   char    *pn_buf;                      0x05b58000
0x7fff0b44   char    *pn_path;                     0x05b58011
0x7fff0b48   int     pn_pathlen;                   0x00000001
0x7fff0b4c };

p4> p s 0x05b58000
0x05b58000
0x05b58000 : /d/pm/meas/wd2bss/

p4> grep -E "from|mntinfo|down" nfhow.txt
mntinfo_t at 0x2c03400
/d/pm/meas from nmsdb:/d/bufds/pm/meas  (Addr 16.40.2.92)
 Flags:
vers=3,proto=udp,auth=unix,hard,printed,intr,down,link,symlink,devs,rsize=32768,wsize=32768,retrans=5
mntinfo_t at 0x2c34c00
/m/nmsetc from nmscom:/d/nmsetc  (Addr 16.40.2.91)
 Flags:
vers=3,proto=udp,auth=unix,hard,printed,intr,down,link,symlink,devs,rsize=32768,wsize=32768,retrans=5
nfs_async_sleep_suspended:          2 (nbr of down mount points)
There are 2 down mount points.

What about the bdf ?

p4> Bdf
Filesystem           kbytes     used    avail %used Mounted on
/dev/root            614400   432158   170900   72% /
/dev/vg00/lvol1       67733    28935    32024   47% /stand
/dev/vg00/lvol6      512000   176710   314402   36% /var
/dev/vg00/lvol5      102400    70601    30482   70% /tmp
/dev/vg00/lvol4      819200   178452   600760   23% /opt
/dev/vgpm/lvol1     8192000  2040329  5816770   26% /d/pm
/dev/vg00/nmsvar     409600     1210   382869    0% /d/nmsvar
/dev/vg00/nmsopt    1228800   459228   721478   39% /d/nmsopt
/dev/vg00/nmsetc     204800    17417   175677    9% /d/nmsetc
/dev/vg00/mail       204800    53861   141512   28% /d/mail
/dev/vg00/home      3375104   147957  3027408    5% /d/home
/dev/vgdb01/db01   16384000 10792246  5247224   67% /d/db01
/dev/vg00/bufmd      819200     1301   766788    0% /d/bufmd
nmsdb:/d/bufds/pm/meas
                          0        0        0    0% /d/pm/meas
nmscom:/d/nmsetc          0        0        0    0% /m/nmsetc

So, that is, 67 csh processes were hanging on the down NFS server
mount point, which leads system behaved as hang.

- For action plan 1, add more swap space.

  At the system hang time, available swap space (swapspc_cnt + swapmem_cnt)
  less than 1MB .

Current swap space configuration

             Kb      Kb      Kb   PCT  START/      Kb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev      524288   45776  478512    9%       0       -    1  /dev/vg00/lvol2
reserve       -  478512 -478512
memory   400820  400776      44  100%

swapmem_on  = 1 
maximum swapmem (swapmem_max)               = 100205 pages (391.43 MB)
remaining swapmem (swapmem_cnt)             = 11 pages (44 KB)
maximum physical swapspace (swapspc_max)    = 131072 pages (512.00 MB)
unreserved physical swapspace (swapspc_cnt) = 0 pages
unused physical swapspace (swapphys_cnt)    = 119628 pages (467.30 MB)

Top 10 Processes sorted by swap size (in pages):

pid   command        virt              phys             swap
---   -------        ----              ----             ----
29404 wd2pnsmx       7633  (29.82 MB)  3890 (15.20 MB)  4154 (16.23 MB)
9568  oracle         17303 (67.59 MB)  2075 (8.11 MB)   3463 (13.53 MB)
29493 wd2nssmx       13762 (53.76 MB)  5504 (21.50 MB)  2746 (10.73 MB)
9570  oracle         16455 (64.28 MB)  2388 (9.33 MB)   2613 (10.21 MB)
9576  oracle         16215 (63.34 MB)  2190 (8.55 MB)   2372 (9.27 MB)
9572  oracle         16215 (63.34 MB)  2093 (8.18 MB)   2372 (9.27 MB)
9578  oracle         16129 (63.00 MB)  2133 (8.33 MB)   2286 (8.93 MB)
9580  oracle         16127 (63.00 MB)  2118 (8.27 MB)   2284 (8.92 MB)
1895  tnslsnr        3259  (12.73 MB)  300  (1.17 MB)   1674 (6.54 MB)
9803  wd2bssmx       5636  (22.02 MB)  881  (3.44 MB)   1570 (6.13 MB)

For action plan 2,
  - Size down kernel parameter dbc_max_pct from 50 to 20,
    to free more memory for application process usage.

   Physical Memory     = 131072 pages (512.00 MB)
   dbc_max_pct           = 50 %
   dbc_min_pct           = 5 %
   dbc current pct       = 45.4 %
   bufpages              = 59518 pages (232.49 MB)
   Number of buf headers = 46420

More infor from crash dump be shown as follows,

			=======================
			= General Information =
			=======================

Dump time Thu May  5 11:26:55 2005 UTC-8
System has been up 126 days, 18 hours, 59 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/803/D320
HP-UX version    : B.11.00 (32-bit Kernel)
Number of CPU's  : 1
Disabled CPU's   : 0
CPU type         : PCXL2 (132 Mhz)
CPU Architecture : PA-RISC 1.1
Load average     : 65.10  64.81  64.80  

			================
			= Crash Events =
			================
This seems to be a user initiated TOC !

Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x21000
= p rpb_t 0x61e268
= Using pc from pim.narrow.rp_pcoq_head = 0xcbcc4
==============  EVENT  ============================
SR4=0x00000000
        SP         RP Return Name
0x005e0530 0x000cbcc4 ms_bit_position_long+0x28
0x005e0530 0x002b01f0 lasi_interrupt+0x24
0x005e04f0 0x0003847c external_interrupt+0x47c
+-------------  TRAP  ----------------------------
|  External interrupt in KERNEL mode at 0xcae04 (idle+0x298)
|  p struct save_state 0.0x5e0030
+-------------  TRAP  ----------------------------
SR4=0x00000000
        SP         RP Return Name
0x01e48170 0x000cae04 idle+0x298
0x01e48030 0x000cccec swidle+0x1c

			==================
			= Memory Globals =
			==================

Physical Memory     = 131072 pages (512.00 MB)
Free Memory         = 7393 pages (28.88 MB)
Average Free Memory = 7405 pages (28.93 MB)
gpgslim             = 1024 pages (4.00 MB)
lotsfree            = 7224 pages (28.22 MB)
desfree             = 1024 pages (4.00 MB)
minfree             = 256 pages (1.00 MB)

			========================
			= Buffer Cache Globals =
			========================

dbc_max_pct           = 50 %
dbc_min_pct           = 5 %
dbc current pct       = 45.4 %
bufpages              = 59518 pages (232.49 MB)
Number of buf headers = 46420

fixed_size_cache = 0 
dbc_parolemem    = 0 
dbc_stealavg     = 1711 
dbc_ceiling      = 65536 pages (256.00 MB)
dbc_nbuf         = 3276 
dbc_bufpages     = 6553 pages (25.60 MB)
dbc_vhandcredit  = 1509 
orignbuf         = 0 
origbufpages     = 0 pages

			====================
			= Swap Information =
			====================

swapinfo -mt emulation
======================

             Mb      Mb      Mb   PCT  START/      Mb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev         512      45     467    9%       0       -    1  LVM vg00/lv2
reserve       -     467    -467
memory      391     391       0  100%
total       903     903       0  100%       -       0    -

swap related globals
====================

swapmem_on  = 1 
maximum swapmem (swapmem_max)               = 100205 pages (391.43 MB)
remaining swapmem (swapmem_cnt)             = 11 pages (44 KB)
maximum physical swapspace (swapspc_max)    = 131072 pages (512.00 MB)
unreserved physical swapspace (swapspc_cnt) = 0 pages
unused physical swapspace (swapphys_cnt)    = 119628 pages (467.30 MB)

Top 10 Processes sorted by swap size (in pages):

pid   command        virt              phys             swap
---   -------        ----              ----             ----
29404 wd2pnsmx       7633  (29.82 MB)  3890 (15.20 MB)  4154 (16.23 MB)
9568  oracle         17303 (67.59 MB)  2075 (8.11 MB)   3463 (13.53 MB)
29493 wd2nssmx       13762 (53.76 MB)  5504 (21.50 MB)  2746 (10.73 MB)
9570  oracle         16455 (64.28 MB)  2388 (9.33 MB)   2613 (10.21 MB)
9576  oracle         16215 (63.34 MB)  2190 (8.55 MB)   2372 (9.27 MB)
9572  oracle         16215 (63.34 MB)  2093 (8.18 MB)   2372 (9.27 MB)
9578  oracle         16129 (63.00 MB)  2133 (8.33 MB)   2286 (8.93 MB)
9580  oracle         16127 (63.00 MB)  2118 (8.27 MB)   2284 (8.92 MB)
1895  tnslsnr        3259  (12.73 MB)  300  (1.17 MB)   1674 (6.54 MB)
9803  wd2bssmx       5636  (22.02 MB)  881  (3.44 MB)   1570 (6.13 MB)

			========================
			= Processor Clock Info =
			========================

hardclock_late = 0
itick_per_tick = 1320000
lbolt          = 1095479779 (0x414bb1e3)

    event mpi        rpb            delta      clk eiem eirr PSW
cpu type  timeinval  interval timer secs:ticks od  0,4  0,4  I
--- ----- ---------- -------------- ---------- --- ---- ---- ---
0   TOC   0xc816843d 0xc8105338          0:0   0   1 1  0 0  1 

			=================
			= Load Averages =
			=================


avenrun
=======
65.10  64.81  64.80  

real_run
========
2.086039  1.793869  1.785234  

pwrun ("fast" io wait)
======================
63.013903  63.016030  63.016286  

mp_avenrun
==========
cpu0 : 65.099942  64.809899  64.801520  

			======================
			= Thread Information =
			======================

20 Threads ran in the last second
32 Threads ran in the last 5 seconds
35 Threads ran in the last 10 seconds
55 Threads ran in the last minute
114 Threads ran in the last hour

statdaemon ran 0 ticks ago


Most Common Wait Channels
=========================
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
*uptr+0                                            78     947806283  85        
nfs3_lookup(0x2c03800)                             63     3255950    1394889   
vx_inactive_thread_sv                              50     99520      1103      
selwait                                            37     1095467310 44        
lvmkd_q                                            6      1905       1905      
svc_run(0x2043ea0)                                 4      1095460218 1095460186
async_bufhead                                      4      3326564    3326564   
ticks_since_boot                                   2      0          0         


Most Common Sleep Callers
=========================
                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
sigsuspend()                                       77     947806283  85        
wait1()                                            66     1095455876 699989    
nfs3_lookup()                                      63     3255950    1394889   
vx_inactive_thread()                               50     99520      1103      
select()                                           26     1095463068 44        
poll()                                             7      1095467310 99        
semop()                                            7      57231      45        
pm_sigwait()                                       7      7245       17        
lvmkd_daemon()                                     6      1905       1905      
svc_run()                                          4      1095460218 1095460186
async_daemon()                                     4      3326564    3326564   
streams_poll()                                     4      1863       265       


Idle Globals
============

candidate_idle_spu = 0
migration_cycles = 0

Running Threads (TSRUNPROC) and idle Processors
===============================================


                    TICKS      TICKS                    I TICKS
                    SINCE      SINCE                    C SINCE      NREADY
TID     PID   PPID  RUN        IDLE       PRI SPU STATE S MIGR       FR LO AL COMMAND
------- ----- ----- ---------- ---------- --- --- ----- - ---------  -- -- -- -------
                               0              0   IDLE  Y 1095479779 0  0  0  

Note: 
FR: free to run on any processor (candidate for thread migration).
LO: locked (via processor affinity/mpctl) to this processor).
AL: Alpha semaphores misses (special scheduling when miss a sema).


Threads waiting on cpu (TSRUN) - sorted by cpu/pri/ticks-since-run
==================================================================

All Threads - sorted by ticks-since-run
=======================================

TID     PID   PPID  TICKS      PRI SPU STAT  SYSCALL        COMMAND        WCHAN
------- ----- ----- ---------- --- --- ----- -------------- -------------- -----
0       0     0     0          128 0   SLEEP n/a            swapper        ticks_since_boot
3       3     0     0          128 0   SLEEP n/a            statdaemon     ticks_since_boot
4       4     0     0          128 0   SLEEP n/a            unhashdaemon   unhash
12      12    0     4          -32 0   SLEEP n/a            ttisr          ttirr
3622    12382 12375 17         168 0   SLEEP sigtimedwait   csh            pm_sigwait(0x61459a0)
7729    9672  9640  44         154 0   SLEEP select         wnemgrmx       selwait
626     569   1     45         127 0   SLEEP read           nktl_daemon    netdiag_ques+0x3c
639     582   1     45         127 0   SLEEP semop          ntl_reader     sema[3][1]+0x4
640     583   582   55         127 0   SLEEP sigtimedwait   netfmt         pm_sigwait(0x3532840)
1396    1339  1     63         154 0   SLEEP select         dced           selwait
7785    9728  9640  82         154 0   SLEEP select         arcintmx       selwait
1806    1749  1     85         120 0   SLEEP sigsuspend     xntpd          *uptr+0
28      28    0     99         138 0   SLEEP n/a            vxfsd          vx_event_wait(0x40f13e0)
29      28    0     99         138 0   SLEEP n/a            vxfsd          vx_iflush_thread_sv
30      28    0     99         138 0   SLEEP n/a            vxfsd          vx_ifree_thread_sv
31      28    0     99         138 0   SLEEP n/a            vxfsd          vx_inactive_cache_thread_sv
1464    1407  1     99         154 0   SLEEP poll           pwgrd          selwait
7625    9568  1     99         156 0   SLEEP semop          oracle         sema[19][2]+0x4
7627    9570  1     99         156 0   SLEEP semop          oracle         sema[19][3]+0x4
7629    9572  1     99         156 0   SLEEP semop          oracle         sema[19][4]+0x4
456     399   1     166        154 0   SLEEP tsync          syncer         default_slice
23664   29404 9640  181        168 0   SLEEP sigtimedwait   wd2pnsmx       pm_sigwait(0x55694c0)
32      28    0     200        138 0   SLEEP n/a            vxfsd          vx_delxwri_thread_sv
7633    9576  1     264        156 0   SLEEP semop          oracle         sema[19][5]+0x4
7736    9679  9640  265        154 0   SLEEP stat           sfwmgrmx       selwait
33      28    0     294        138 0   SLEEP n/a            vxfsd          vx_logflush_thread_sv
7697    9640  1     295        168 0   SLEEP sigtimedwait   wpmanamx       pm_sigwait(0x400e6c0)
598     541   1     296        154 0   SLEEP select         syslogd        selwait
1582    1525  1     298        154 0   SLEEP select         diagmond       selwait
1960    1903  1525  417        158 0   SLEEP read           diaglogd       diag2_sleep_flag
1962    1905  1525  418        154 0   SLEEP select         psmctd         selwait
7745    9688  9640  429        168 0   SLEEP sigtimedwait   wd2pbsmx       pm_sigwait(0x3b2f9e0)
2       2     0     502        128 0   SLEEP n/a            vhand          vhandsema
23753   29493 9640  595        154 0   SLEEP open           wd2nssmx       selwait
5224    13984 1489  711        168 0   SLEEP pause          sh             *uptr+0
34      28    0     1103       138 0   SLEEP n/a            vxfsd          vx_attr_thread_sv
35      28    0     1103       138 0   SLEEP n/a            vxfsd          vx_tuning_thread_sv
49      28    0     1103       138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
1599    1542  1     1488       154 0   SLEEP select         psmond         selwait
7860    9803  9640  1498       154 0   SLEEP open           wd2bssmx       selwait
26634   5526  5522  1863       154 0   SLEEP stat           csh            selwait
18      18    0     1905       147 0   SLEEP n/a            lvmkd          lvmkd_q
19      19    0     1905       147 0   SLEEP n/a            lvmkd          lvmkd_q
20      20    0     1905       147 0   SLEEP n/a            lvmkd          lvmkd_q
21      21    0     1905       147 0   SLEEP n/a            lvmkd          lvmkd_q
22      22    0     1905       147 0   SLEEP n/a            lvmkd          lvmkd_q
23      23    0     1905       147 0   SLEEP n/a            lvmkd          lvmkd_q
1546    1489  1     2122       168 0   SLEEP sigtimedwait   cron           pm_sigwait(0x61f6be0)
1336    1279  1     2645       154 0   SLEEP select         mib2agt        selwait
85      28    0     3102       138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
1655    1598  1     3974       154 0   SLEEP select         swagentd       selwait
1256    1199  1     4844       154 0   SLEEP select         sendmail       selwait
2457    2400  1     5013       154 0   SLEEP select         dm_stape       selwait
66      28    0     5119       138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
891     834   1     5183       154 0   SLEEP select         automount      selwait
47      28    0     7120       138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
943     886   1     7144       154 0   SLEEP select         inetd          selwait
5129    13889 1     7245       168 0   SLEEP sigtimedwait   p_client       pm_sigwait(0x5c02f80)
7635    9578  1     7859       156 0   SLEEP semop          oracle         sema[19][6]+0x4
70      28    0     9128       138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
5210    13970 1     10499      156 0   SLEEP read           getty          DEVICE 0x00000000
1       1     0     10621      168 0   SLEEP sigsuspend     init           *uptr+0
53      28    0     11145      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
62      28    0     13153      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
2424    2367  1     14615      154 0   SLEEP select         sysstat_em     selwait
73      28    0     15152      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
77      28    0     17171      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
63      28    0     19181      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
40      28    0     21180      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
72      28    0     23197      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
56      28    0     25207      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
57      28    0     27208      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
64      28    0     29223      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
79      28    0     31233      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
45      28    0     33233      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
83      28    0     35236      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
2305    2248  1     36385      154 0   SLEEP select         dm_core_hw     selwait
74      28    0     37261      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
58      28    0     39263      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
41      28    0     41275      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
51      28    0     43289      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
44      28    0     45291      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
67      28    0     47299      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
61      28    0     49303      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
42      28    0     51313      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
36      28    0     53326      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
39      28    0     55339      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
7637    9580  1     57231      156 0   SLEEP semop          oracle         sema[19][7]+0x4
46      28    0     57339      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
84      28    0     59353      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
68      28    0     61363      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
81      28    0     63365      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
80      28    0     65383      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
78      28    0     67390      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
37      28    0     69391      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
71      28    0     71406      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
75      28    0     73415      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
65      28    0     75415      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
54      28    0     77432      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
76      28    0     79442      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
38      28    0     81443      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
55      28    0     83458      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
59      28    0     85468      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
48      28    0     87469      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
69      28    0     89484      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
82      28    0     91494      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
2052    1995  1     93355      154 0   SLEEP select         disk_em        selwait
50      28    0     93493      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
60      28    0     95510      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
43      28    0     97520      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
52      28    0     99520      138 0   SLEEP n/a            vxfsd          vx_inactive_thread_sv
2382    2325  1     127207     154 0   SLEEP select         dm_memory      selwait
819     762   1     261101     154 0   SLEEP select         ypbind         selwait
1961    1904  1525  295309     154 0   SLEEP select         memlogd        selwait
3615    12375 1489  699989     158 0   SLEEP waitpid        sh             proc[295]
735     678   1     712000     154 0   SLEEP poll           rpcbind        selwait
2286    11052 11048 1394889    168 0   SLEEP sigsuspend     csh            *uptr+0
2291    11057 11052 1394889    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
2282    11048 1489  1395922    158 0   SLEEP waitpid        sh             proc[286]
2270    11036 11032 1400946    168 0   SLEEP sigsuspend     csh            *uptr+0
2279    11045 11036 1400946    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
2266    11032 1489  1401982    158 0   SLEEP waitpid        sh             proc[271]
2216    10982 10978 1455896    168 0   SLEEP sigsuspend     csh            *uptr+0
2220    10986 10982 1455896    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
2212    10978 1489  1455932    158 0   SLEEP waitpid        sh             proc[280]
2204    10970 10967 1461892    168 0   SLEEP sigsuspend     csh            *uptr+0
2208    10974 10970 1461892    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
2201    10967 1489  1461928    158 0   SLEEP waitpid        sh             proc[276]
1919    10695 10691 1515875    168 0   SLEEP sigsuspend     csh            *uptr+0
1923    10699 10695 1515875    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1915    10691 1489  1515908    158 0   SLEEP waitpid        sh             proc[263]
1907    10683 10680 1521866    168 0   SLEEP sigsuspend     csh            *uptr+0
1911    10687 10683 1521867    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1904    10680 1489  1521903    158 0   SLEEP waitpid        sh             proc[278]
1832    10608 10604 1575942    168 0   SLEEP sigsuspend     csh            *uptr+0
1836    10612 10608 1575942    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1828    10604 1489  1575977    158 0   SLEEP waitpid        sh             proc[270]
1820    10596 10593 1581939    168 0   SLEEP sigsuspend     csh            *uptr+0
1824    10600 10596 1581939    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1817    10593 1489  1581975    158 0   SLEEP waitpid        sh             proc[267]
1524    10310 10308 1635896    168 0   SLEEP sigsuspend     csh            *uptr+0
1528    10314 10310 1635896    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1522    10308 1489  1635931    158 0   SLEEP waitpid        sh             proc[260]
1513    10299 10296 1641895    168 0   SLEEP sigsuspend     csh            *uptr+0
1517    10303 10299 1641895    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1510    10296 1489  1641929    158 0   SLEEP waitpid        sh             proc[256]
1469    10255 10252 1695900    168 0   SLEEP sigsuspend     csh            *uptr+0
1473    10259 10255 1695900    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1466    10252 1489  1695932    158 0   SLEEP waitpid        sh             proc[253]
1456    10243 10240 1701892    168 0   SLEEP sigsuspend     csh            *uptr+0
1460    10247 10243 1701892    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1453    10240 1489  1701930    158 0   SLEEP waitpid        sh             proc[259]
1163    9956  9953  1755926    168 0   SLEEP sigsuspend     csh            *uptr+0
1167    9960  9956  1755926    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1160    9953  1489  1755962    158 0   SLEEP waitpid        sh             proc[248]
1146    9939  9936  1761890    168 0   SLEEP sigsuspend     csh            *uptr+0
1153    9946  9939  1761890    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1143    9936  1489  1761932    158 0   SLEEP waitpid        sh             proc[249]
1105    9898  9895  1815897    168 0   SLEEP sigsuspend     csh            *uptr+0
1109    9902  9898  1815897    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1102    9895  1489  1815932    158 0   SLEEP waitpid        sh             proc[246]
1093    9886  9883  1821896    168 0   SLEEP sigsuspend     csh            *uptr+0
1097    9890  9886  1821896    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
1090    9883  1489  1821932    158 0   SLEEP waitpid        sh             proc[242]
812     9607  9604  1875915    168 0   SLEEP sigsuspend     csh            *uptr+0
816     9611  9607  1875915    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
809     9604  1489  1875947    158 0   SLEEP waitpid        sh             proc[240]
799     9595  9592  1881905    168 0   SLEEP sigsuspend     csh            *uptr+0
804     9599  9595  1881906    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
796     9592  1489  1881944    158 0   SLEEP waitpid        sh             proc[237]
722     9513  9510  1935892    168 0   SLEEP sigsuspend     csh            *uptr+0
726     9517  9513  1935892    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
719     9510  1489  1935925    158 0   SLEEP waitpid        sh             proc[232]
710     9500  9497  1941887    168 0   SLEEP sigsuspend     csh            *uptr+0
714     9504  9500  1941887    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
707     9497  1489  1941923    158 0   SLEEP waitpid        sh             proc[230]
427     9221  9218  1995946    168 0   SLEEP sigsuspend     csh            *uptr+0
431     9225  9221  1995946    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
424     9218  1489  1995980    158 0   SLEEP waitpid        sh             proc[226]
415     9209  9206  2001944    168 0   SLEEP sigsuspend     csh            *uptr+0
419     9213  9209  2001944    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
412     9206  1489  2001978    158 0   SLEEP waitpid        sh             proc[223]
367     9161  9157  2055874    168 0   SLEEP sigsuspend     csh            *uptr+0
371     9165  9161  2055874    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
363     9157  1489  2055911    158 0   SLEEP waitpid        sh             proc[213]
354     9148  9145  2061866    168 0   SLEEP sigsuspend     csh            *uptr+0
358     9152  9148  2061866    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
351     9145  1489  2061902    158 0   SLEEP waitpid        sh             proc[220]
29968   8862  8858  2115923    168 0   SLEEP sigsuspend     csh            *uptr+0
29972   8866  8862  2115923    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29964   8858  1489  2115959    158 0   SLEEP waitpid        sh             proc[211]
29951   8845  8841  2121888    168 0   SLEEP sigsuspend     csh            *uptr+0
29957   8851  8845  2121888    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29947   8841  1489  2121929    158 0   SLEEP waitpid        sh             proc[212]
29914   8808  8804  2175868    168 0   SLEEP sigsuspend     csh            *uptr+0
29918   8812  8808  2175868    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29910   8804  1489  2175905    158 0   SLEEP waitpid        sh             proc[203]
29901   8795  8792  2181878    168 0   SLEEP sigsuspend     csh            *uptr+0
29905   8799  8795  2181878    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29898   8792  1489  2181914    158 0   SLEEP waitpid        sh             proc[202]
29620   8515  8512  2235883    168 0   SLEEP sigsuspend     csh            *uptr+0
29624   8519  8515  2235883    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29617   8512  1489  2235916    158 0   SLEEP waitpid        sh             proc[193]
29607   8502  8499  2241877    168 0   SLEEP sigsuspend     csh            *uptr+0
29611   8506  8502  2241877    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29604   8499  1489  2241914    158 0   SLEEP waitpid        sh             proc[201]
29534   8429  8426  2295956    168 0   SLEEP sigsuspend     csh            *uptr+0
29538   8433  8429  2295956    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29531   8426  1489  2295988    158 0   SLEEP waitpid        sh             proc[198]
29525   8420  8416  2301939    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29521   8416  8413  2301943    168 0   SLEEP sigsuspend     csh            *uptr+0
29518   8413  1489  2301984    158 0   SLEEP waitpid        sh             proc[195]
29239   8133  8130  2355884    168 0   SLEEP sigsuspend     csh            *uptr+0
29243   8137  8133  2355884    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29236   8130  1489  2355919    158 0   SLEEP waitpid        sh             proc[183]
29226   8120  8117  2361879    168 0   SLEEP sigsuspend     csh            *uptr+0
29230   8124  8120  2361880    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29223   8117  1489  2361916    158 0   SLEEP waitpid        sh             proc[189]
29178   8072  8069  2415919    168 0   SLEEP sigsuspend     csh            *uptr+0
29182   8076  8072  2415919    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29175   8069  1489  2415951    158 0   SLEEP waitpid        sh             proc[186]
29165   8059  8056  2421916    168 0   SLEEP sigsuspend     csh            *uptr+0
29169   8063  8059  2421916    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
29162   8056  1489  2421953    158 0   SLEEP waitpid        sh             proc[185]
28880   7771  7768  2475940    168 0   SLEEP sigsuspend     csh            *uptr+0
28884   7775  7771  2475940    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28877   7768  1489  2475977    158 0   SLEEP waitpid        sh             proc[173]
28863   7754  7751  2481899    168 0   SLEEP sigsuspend     csh            *uptr+0
28870   7761  7754  2481899    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28860   7751  1489  2481945    158 0   SLEEP waitpid        sh             proc[167]
28823   7714  7711  2535892    168 0   SLEEP sigsuspend     csh            *uptr+0
28827   7718  7714  2535892    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28820   7711  1489  2535927    158 0   SLEEP waitpid        sh             proc[174]
28810   7701  7698  2541894    168 0   SLEEP sigsuspend     csh            *uptr+0
28814   7705  7701  2541894    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28807   7698  1489  2541930    158 0   SLEEP waitpid        sh             proc[168]
28530   7421  7418  2595899    168 0   SLEEP sigsuspend     csh            *uptr+0
28534   7425  7421  2595899    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28527   7418  1489  2595931    158 0   SLEEP waitpid        sh             proc[161]
28517   7408  7405  2601893    168 0   SLEEP sigsuspend     csh            *uptr+0
28521   7412  7408  2601893    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28514   7405  1489  2601930    158 0   SLEEP waitpid        sh             proc[159]
28444   7335  7332  2655887    168 0   SLEEP sigsuspend     csh            *uptr+0
28448   7339  7335  2655887    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28441   7332  1489  2655920    158 0   SLEEP waitpid        sh             proc[158]
28431   7322  7319  2661882    168 0   SLEEP sigsuspend     csh            *uptr+0
28435   7326  7322  2661882    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28428   7319  1489  2661918    158 0   SLEEP waitpid        sh             proc[146]
28149   7040  7037  2715957    168 0   SLEEP sigsuspend     csh            *uptr+0
28153   7044  7040  2715957    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28146   7037  1489  2715991    158 0   SLEEP waitpid        sh             proc[156]
28136   7027  7024  2721954    168 0   SLEEP sigsuspend     csh            *uptr+0
28140   7031  7027  2721955    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28133   7024  1489  2721988    158 0   SLEEP waitpid        sh             proc[153]
28096   6987  6984  2775941    168 0   SLEEP sigsuspend     csh            *uptr+0
28100   6991  6987  2775941    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28093   6984  1489  2775974    158 0   SLEEP waitpid        sh             proc[142]
28082   6973  6970  2781938    168 0   SLEEP sigsuspend     csh            *uptr+0
28086   6977  6973  2781938    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
28079   6970  1489  2781974    158 0   SLEEP waitpid        sh             proc[150]
27799   6690  6687  2835950    168 0   SLEEP sigsuspend     csh            *uptr+0
27803   6694  6690  2835951    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27796   6687  1489  2835983    158 0   SLEEP waitpid        sh             proc[145]
27783   6674  6671  2841910    168 0   SLEEP sigsuspend     csh            *uptr+0
27790   6681  6674  2841910    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27780   6671  1489  2841953    158 0   SLEEP waitpid        sh             proc[140]
27711   6602  6599  2895943    168 0   SLEEP sigsuspend     csh            *uptr+0
27715   6606  6602  2895943    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27708   6599  1489  2895975    158 0   SLEEP waitpid        sh             proc[93]
27698   6589  6586  2901936    168 0   SLEEP sigsuspend     csh            *uptr+0
27702   6593  6589  2901937    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27695   6586  1489  2901975    158 0   SLEEP waitpid        sh             proc[136]
27427   6318  6315  2955911    168 0   SLEEP sigsuspend     csh            *uptr+0
27431   6322  6318  2955911    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27424   6315  1489  2955943    158 0   SLEEP waitpid        sh             proc[134]
27413   6304  6301  2961900    168 0   SLEEP sigsuspend     csh            *uptr+0
27417   6308  6304  2961901    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27410   6301  1489  2961945    158 0   SLEEP waitpid        sh             proc[80]
27340   6231  6228  3015890    168 0   SLEEP sigsuspend     csh            *uptr+0
27344   6235  6231  3015891    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27337   6228  1489  3015929    158 0   SLEEP waitpid        sh             proc[130]
27327   6218  6215  3021888    168 0   SLEEP sigsuspend     csh            *uptr+0
27331   6222  6218  3021888    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27324   6215  1489  3021924    158 0   SLEEP waitpid        sh             proc[94]
27049   5941  5937  3075915    168 0   SLEEP sigsuspend     csh            *uptr+0
27053   5945  5941  3075915    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27045   5937  1489  3075952    158 0   SLEEP waitpid        sh             proc[131]
27035   5927  5924  3081912    168 0   SLEEP sigsuspend     csh            *uptr+0
27039   5931  5927  3081912    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
27032   5924  1489  3081952    158 0   SLEEP waitpid        sh             proc[104]
26994   5886  5883  3135906    168 0   SLEEP sigsuspend     csh            *uptr+0
26998   5890  5886  3135906    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
26991   5883  1489  3135938    158 0   SLEEP waitpid        sh             proc[112]
26981   5873  5870  3141902    168 0   SLEEP sigsuspend     csh            *uptr+0
26985   5877  5873  3141903    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
26978   5870  1489  3141939    158 0   SLEEP waitpid        sh             proc[105]
26696   5588  5585  3195906    168 0   SLEEP sigsuspend     csh            *uptr+0
26700   5592  5588  3195906    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
26693   5585  1489  3195938    158 0   SLEEP waitpid        sh             proc[107]
26680   5572  5569  3201867    168 0   SLEEP sigsuspend     csh            *uptr+0
26687   5579  5572  3201867    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
26677   5569  1489  3201907    158 0   SLEEP waitpid        sh             proc[76]
26642   5534  5531  3255950    168 0   SLEEP sigsuspend     csh            *uptr+0
26646   5538  5534  3255950    138 0   SLEEP stat           csh            nfs3_lookup(0x2c03800)
26639   5531  1489  3255984    158 0   SLEEP waitpid        sh             proc[69]
26630   5522  5517  3261926    168 0   SLEEP sigsuspend     csh            *uptr+0
26625   5517  1489  3261963    158 0   SLEEP waitpid        sh             proc[126]
1290    1233  1     3314611    154 0   SLEEP select         snmpdm         selwait
1952    1895  1     3318945    154 0   SLEEP select         tnslsnr        selwait
863     806   1     3326564    154 0   SLEEP async_daemon   biod           async_bufhead
864     807   1     3326564    154 0   SLEEP async_daemon   biod           async_bufhead
865     808   1     3326564    154 0   SLEEP async_daemon   biod           async_bufhead
866     809   1     3326564    154 0   SLEEP async_daemon   biod           async_bufhead
8       8     0     3390178    100 0   SLEEP n/a            supsched       streams_up_runq
2006    10490 1     24088924   168 0   SLEEP sigsuspend     csh            *uptr+0
2050    1993  1977  126963002  154 0   SLEEP select         dtlogin        selwait
3598    9506  1     223879853  168 0   SLEEP sigsuspend     csh            *uptr+0
21119   26958 1     229819731  168 0   SLEEP sigsuspend     csh            *uptr+0
15229   21032 1     232879632  168 0   SLEEP sigsuspend     csh            *uptr+0
19459   24643 1     296956652  168 0   SLEEP sigsuspend     csh            *uptr+0
27080   1321  1     417371079  168 0   SLEEP sigsuspend     csh            *uptr+0
21586   23673 1     668279367  168 0   SLEEP sigsuspend     csh            *uptr+0
1978    1921  886   689166783  154 0   SLEEP select         registrar      selwait
24909   26197 1     790493418  168 0   SLEEP sigsuspend     csh            *uptr+0
6914    7857  1     849710836  168 0   SLEEP sigsuspend     csh            *uptr+0
25930   26602 1     900828274  168 0   SLEEP sigsuspend     csh            *uptr+0
29792   357   1     947806283  168 0   SLEEP sigsuspend     csh            *uptr+0
2034    1977  1     1095455876 158 0   SLEEP waitpid        dtrc           proc[81]
1724    1667  1     1095460067 154 0   SLEEP poll           rpc.mountd     selwait
1731    1674  1     1095460186 154 0   SLEEP nfssvc         nfsd           svc_run(0x2043ea0)
1739    1682  1674  1095460211 154 0   SLEEP nfssvc         nfsd           svc_run(0x2043ea0)
1738    1681  1674  1095460212 154 0   SLEEP nfssvc         nfsd           svc_run(0x2043ea0)
1736    1679  1674  1095460218 154 0   SLEEP nfssvc         nfsd           svc_run(0x2043ea0)
1689    1632  1     1095460354 154 0   SLEEP select         emsagent       selwait
880     823   1     1095460687 154 0   SLEEP poll           rpc.lockd      selwait
1632    1575  1     1095460695 154 0   SLEEP poll           SLSd_daemon    selwait
1610    1553  1     1095460736 154 0   SLEEP read           envd           FIFO 0x2c37a00
1350    1293  1     1095462925 154 0   SLEEP select         trapdestagt    selwait
1322    1265  1     1095463068 154 0   SLEEP select         hp_unixagt     selwait
874     817   1     1095465692 154 0   SLEEP poll           rpc.statd      selwait
800     743   1     1095467310 154 0   SLEEP poll           keyserv        selwait
740     683   0     1095467563 152 0   SLEEP n/a            nfskd          num_buffers_active+0x20
607     550   1     1095468847 155 0   SLEEP msgrcv         ptydaemon      msgque[0]+0x38
9       9     0     1095476760 100 0   SLEEP n/a            strmem         edata+0x3e4
10      10    0     1095476760 100 0   SLEEP n/a            strweld        weldq_runq
11      11    0     1095476760 100 0   SLEEP n/a            strfreebd      str_freeb_idle

			==================
			= Kernel Patches =
			==================

PHKL_12965	PHKL_13431	PHKL_13552	PHKL_13810	PHKL_14088
PHKL_14292	PHKL_14750	PHKL_14762	PHKL_14763	PHKL_14765
PHKL_15510	PHKL_15547	PHKL_15550	PHKL_15551	PHKL_15553
PHKL_15554	PHKL_15689	PHKL_15705	PHKL_15910	PHKL_16074
PHKL_16209	PHKL_16236	PHKL_16807	PHKL_16819	PHKL_16876
PHKL_16983	PHKL_17038	PHKL_17042	PHKL_17205	PHKL_17258
PHKL_17458	PHKL_17801	PHKL_17869	PHKL_17953	PHKL_18111
PHKL_18141	PHKL_18295	PHKL_18452	PHKL_18483	PHKL_18531
PHKL_18533	PHKL_18542	PHKL_18543	PHKL_18798	PHKL_18799
PHKL_18800	PHKL_19080	PHKL_19135	PHKL_19169	PHKL_19201
PHKL_19202	PHKL_19203	PHKL_19204	PHKL_19311	PHKL_19321
PHKL_19800	PHKL_19909	PHKL_20016	PHKL_20017	PHKL_20123
PHKL_20139	PHKL_20151	PHKL_20157	PHKL_20158	PHKL_20159
PHKL_20161	PHKL_20162	PHKL_20163	PHKL_20165	PHKL_20168
PHKL_20170	PHKL_20171	PHKL_20172	PHKL_20173	PHKL_20176
PHKL_20186	PHKL_20222	PHKL_20223	PHKL_20224	PHKL_20225
PHKL_20226	PHKL_20228	PHKL_20229	PHKL_20333	PHKL_20473
PHKL_20836	PHKL_20995	PHNE_13417	PHNE_13543	PHNE_14620
PHNE_14919	PHNE_14976	PHNE_15414	PHNE_15415	PHNE_15537
PHNE_16017	PHNE_16155	PHNE_16470	PHNE_16546	PHNE_16599
PHNE_17586	PHNE_18272	PHNE_18409	PHNE_18878	PHNE_18972
PHNE_19081	PHNE_19754	PHNE_19899	PHNE_20431	PHNE_21257
PHNE_27393	PHNE_27788	

```

