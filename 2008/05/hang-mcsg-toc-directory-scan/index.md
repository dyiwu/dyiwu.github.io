# MC/SG TOC system due to heavy CPU loading on directory scan.

## MC/SG TOC system due to heavy CPU loading on directory scan.

NFSv3 server nfsktcpd (multi-threaded nfs/tcp server kernel daemon)
takes much CPU times and following MC/SG TOC system due to safety timer
timedout.

```

System crash dump analysis report
=================================
Symptom
=======
  NFSv3 server nfsktcpd (multi-threaded nfs/tcp server kernel daemon)
  takes much CPU times and following MC/SG TOC system due to safety timer
  timedout.

Root cause
==========
  Cmcld is starved for cpu, but cpu spends too much time in vx_dirscan()

Action plan
===========
  Directory rearrangement (to reduce number of files in each directories)
  are suggested.  (/dc_home/[mntpt]/ft/summary/fleft/summary)

Detail analysis
===============
			=======================
			= General Information =
			=======================

Dump time Sat May 17 12:33:17 2008 UTC-8
System has been up 1 day, 15 hours, 46 minutes.

Node Name         : localhost
Model             : server rx2620
BIOS revision     : 04.27
HP-UX version     : B.11.23
Kernel whatstring : @(#) $Revision: vmunix:    
                    B11.23_LR FLAVOR=perf Fri Aug 29 22:35:38 PDT 2003 $
Number of CPU's   : 2
Disabled CPU's    : 0
CPU type          : IA64 (1.6 Ghz)
CPU Architecture  : IA-64 0
Load average      : 36.88  21.37  10.69  

			================
			= Crash Events =
			================

This is a Service Guard INIT !

Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is CT_INIT on CPU #0;
= p crash_event_t 0xe000000100007000
= p rpb_t 0xe000000100fb8090
= Process at 0xe00000012d752000, pid 10, "nfsktcpd"
==============  EVENT  ============================
RR0=0x00ffff31  RR1=0x00ffff31  RR2=0x00ffff31  RR3=0x00ffff31
RR4=0x07ff0031  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0x9fffffff7f729490 0x9fffffff7f7274d0 0xe00000000068d211 vx_dirscan+0x4d1
0x9fffffff7f7293b0 0x9fffffff7f727510 0xe000000000664e80 vx_dirlook+0x360
0x9fffffff7f729300 0x9fffffff7f727530 0xe000000000633c90 vx_lookup+0x310
0x9fffffff7f729270 0x9fffffff7f727550 0xe0000000012e5050 rfs3_lookup+0x190
0x9fffffff7f729198 0x9fffffff7f727690 0xe0000000012f1a00 rfsexp_dispatch+0x460
0x9fffffff7f729100 0x9fffffff7f727980 0xe000000001428130 tcp_svc_getreq+0x1f0
0x9fffffff7f729010 0x9fffffff7f727a30 0xe000000000733c80 tcp_svc_run+0x3e0
0x9fffffff7f729000 0x9fffffff7f727bf0 0xe0000000013c6f50 kthread_daemon_startup+0x40
------------------------------------------------------
tid = 279095  kt_stat = TSRUN
local operation on vnodep = 0xe00000016f6705c0
 (/dc_home/[mntpt]/ft/summary/fleft/summary)

local vnode belongs to vfsp = 0xe00000012a8ddac0  mnt_pt = '/dc_home'

local component being looked up =
 'FLEX04_SC74899ADW_Hot_FT_BRX_SC74899ADW_W_HOT_XG632AQU00_FINAL_TEST1_95'C_Aug19_21_33_SUM.txt.Z'
SVCXPRT used = 0xe000000163ff2040
NFS filehandle corresponds to inode = 14534 of filesystem '/dc_home' 
vfsp = 0xe00000012a8ddac0
--------------------------------------------------------

Processor #1

==============  EVENT  ============================
= Event #1 is CT_INIT on CPU #1;
= p crash_event_t 0xe000000100007030
= p rpb_t 0xe0000001015fb4c0
= Process at 0xe00000012d752000, pid 10, "nfsktcpd"
==============  EVENT  ============================
RR0=0x00ffff31  RR1=0x00ffff31  RR2=0x00ffff31  RR3=0x00ffff31
RR4=0x07ff0031  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0xe0000001017c47e8 0xe0000001017d7ae0 0xe000000001465df0 send_INIT_monarch+0x70
0xe0000001017c47d0 0xe0000001017d7ae0 0xe000000001465e40 Send_Monarch_TOC+0x40
0xe0000001017c4760 0xe0000001017d7ae0 0xe000000001062b20 safety_time_check+0x280
0xe0000001017c4740 0xe0000001017d7b10 0xe0000000006acb80 perform_clock_ints+0x60
0xe0000001017c4630 0xe0000001017d7b10 0xe0000000006acd90 per_spu_hardclock+0x1d0
0xe0000001017c4500 0xe0000001017d7b60 0xe0000000006aeae0 clock_int+0xd20
0xe0000001017c43e8 0xe0000001017d7bc0 0xe000000000800610 external_interrupt+0x3b0
0xe0000001017c43c0 0xe0000001017d7bf0 0xe000000000c2c780 bubbleup+0x880
+-------------  TRAP  ----------------------------
|  External Interrupt in KERNEL mode
|    IIP=0xe0000000016aa0b0:0
|  p struct save_state 0xdead31.0xe0000001017d7c00
+-------------  TRAP  ----------------------------
               BSP                 SP                 IP
0x9fffffff7f7c9af0 0x9fffffff7f7c7310 0xe0000000016aa0b0 vx_bread_typed+0x80
0x9fffffff7f7c9978 0x9fffffff7f7c7310 0xe00000000077dda0 vx_do_bmap_typed+0xaa0
0x9fffffff7f7c9808 0x9fffffff7f7c7340 0xe00000000077dd30 vx_do_bmap_typed+0xa30
0x9fffffff7f7c97c0 0x9fffffff7f7c7370 0xe00000000079e1e0 vx_bmap_typed+0xc0
0x9fffffff7f7c9738 0x9fffffff7f7c7410 0xe000000000629120 vx_bmap_lockheld2+0x120
0x9fffffff7f7c96b8 0x9fffffff7f7c7410 0xe00000000062e0c0 vx_bmap+0x140
0x9fffffff7f7c95d8 0x9fffffff7f7c7430 0xe0000000006894a0 vx_dirbread+0xb0
0x9fffffff7f7c9490 0x9fffffff7f7c74d0 0xe00000000068d800 vx_dirscan+0xac0
0x9fffffff7f7c93b0 0x9fffffff7f7c7510 0xe000000000664e80 vx_dirlook+0x360
0x9fffffff7f7c9300 0x9fffffff7f7c7530 0xe000000000633c90 vx_lookup+0x310
0x9fffffff7f7c9270 0x9fffffff7f7c7550 0xe0000000012e5050 rfs3_lookup+0x190
0x9fffffff7f7c9198 0x9fffffff7f7c7690 0xe0000000012f1a00 rfsexp_dispatch+0x460
0x9fffffff7f7c9100 0x9fffffff7f7c7980 0xe000000001428130 tcp_svc_getreq+0x1f0
0x9fffffff7f7c9010 0x9fffffff7f7c7a30 0xe000000000733c80 tcp_svc_run+0x3e0
0x9fffffff7f7c9000 0x9fffffff7f7c7bf0 0xe0000000013c6f50 kthread_daemon_startup+0x40
--------------------------------------------------------
tid = 278662  kt_stat = TSRUN
local operation on vnodep = 0xe00000016f6705c0 
(/dc_home/[mntpt]/ft/summary/fleft/summary)
local vnode belongs to vfsp = 0xe00000012a8ddac0  mnt_pt = '/dc_home'
local component being looked up = 
'FLEX07-CSR_1_bc4_pugwash_f_3v06a_x8_LCHS1-F_25C_RT0_GP5530XAAM_FT_GP5530.1_FT_Jan10_09_02.summary.Z'
SVCXPRT used = 0xe000000163ff2040
NFS filehandle corresponds to inode = 14534 of filesystem '/dc_home' 
vfsp = 0xe00000012a8ddac0
--------------------------------------------------------

=================
= Load Averages =
=================

avenrun
=======
36.88  21.37  10.69  

mp_avenrun
==========
cpu0 : 37.311754  21.748574  10.940800  
cpu1 : 40.747521  22.928449  11.411689  

Running Threads (TSRUNPROC) 
===========================

                   TICKS TICKS                I TICKS
                   SINCE SINCE                C SINCE NREADY
TID    PID   PPID  RUN   IDLE  PRI  SPU STATE S MIGR  FR LO AL COMMAND
------ ----- ----- ----- ----- ---- --- ----- - ----- -- -- -- -------
279095 10    0     4415  4509  143  0   SYS   N 4509  43 4  0  nfsktcpd
278662 10    0     5187  16415 138  1   SYS   Y 16416 44 2  0  nfsktcpd

Note: 
FR: free to run on any processor (candidate for thread migration).
LO: locked (via processor affinity/mpctl) to this processor).
AL: Alpha semaphores misses (special scheduling when miss a sema).

Threads waiting on cpu (TSRUN) - sorted by cpu/pri/ticks-since-run
==================================================================

TID    PID   PPID  TICKS    PRI  SPU BND  GRP SYSCALL        COMMAND
------ ----- ----- -------- ---- --- ---- --- -------------- -------
45     29    0     5192     -32  0   N    -1  n/a            ipmid
3395   3053  3052  4415     -27  0   N    -1  sigwait        cmcld
3398   3053  3052  4515     -27  0   N    -1  select         cmcld
1590   1476  1     4415     -16  0   LDOM -1  ki_call        midaemon
3397   3057  3056  4415     -14  0   N    -1  select         cmlogd
3399   3053  3052  4515     -14  0   N    -1  select         cmcld
57     40    0     5194     100  0   N    -1  n/a            smpsched
1442   1328  1     4415     120  0   N    -1  sigsuspend     xntpd
0      0     0     4415     127  0   N    -1  n/a            swapper
724    641   640   4510     127  0   N    -1  sigtimedwait   netfmt
1641   1515  1     5190     127  0   N    -1  write          scopeux
3      3     0     4415     128  0   N    -1  n/a            statdaemon
4      4     0     4415     128  0   N    -1  n/a            unhashdaemon
63     37    0     4510     128  0   N    -1  n/a            schedcpu
64     37    0     4510     128  0   N    -1  n/a            schedcpu
483    19    0     4415     133  0   CPU  0   n/a            ksyncer_daemon
36     20    0     5193     133  0   N    -1  n/a            lvmdevd
79     51    0     4415     134  0   N    -1  n/a            vxfsd
84     51    0     4415     134  0   N    -1  n/a            vxfsd
278661 10    0     4509     134  0   N    -1  n/a            nfsktcpd
35     19    0     5193     134  0   N    -1  n/a            ksyncer_daemon
279967 10321 10320 5191     137  0   N    -1  ksleep         java
75     51    0     4415     138  0   N    -1  n/a            vxfsd
280253 51    0     16415    138  0   N    -1  n/a            vxfsd
280256 51    0     22515    138  0   N    -1  n/a            vxfsd
280258 51    0     22515    138  0   N    -1  n/a            vxfsd
14     12    0     4510     147  0   N    -1  n/a            lvmkd
15     13    0     4510     147  0   N    -1  n/a            lvmkd
16     14    0     4510     147  0   N    -1  n/a            lvmkd
17     15    0     4510     147  0   N    -1  n/a            lvmkd
18     16    0     4510     147  0   N    -1  n/a            lvmkd
19     17    0     4510     147  0   N    -1  n/a            lvmkd
20     18    0     4510     148  0   N    -1  n/a            lvmschedd
12921  7440  0     4515     148  0   N    -1  n/a            lvmdevd
10     10    0     4515     152  0   N    -1  n/a            nfsktcpd
55     39    0     4515     152  0   N    -1  n/a            cmcd
502    421   1     4511     154  0   N    0   ksleep         utmpd
1632   1507  1     4512     154  0   N    -1  select         spagent
2202   1870  1863  4512     154  0   N    -1  ksleep         agent
1513   1399  1     4513     154  0   N    -1  select         diagmond
1395   1303  1     5193     154  0   N    -1  ksleep         dced
2637   2307  1485  5193     154  0   N    -1  ksleep         rep_server
2670   2362  1485  5193     154  0   N    -1  ksleep         agdbserver
2402   2129  1399  4515     158  0   N    -1  read           diaglogd
19987  10321 10320 4509     168  0   N    -1  nanosleep      java
20346  10387 10386 4509     168  0   N    -1  nanosleep      java
20359  10387 10386 4509     168  0   N    -1  nanosleep      java
500    421   1     4512     168  0   N    0   sigtimedwait   utmpd
2061   1881  1     4514     168  0   N    -1  sigtimedwait   cimserverd
653    572   1     4515     168  0   N    -1  sigtimedwait   ipmon
1276   1192  1     4515     168  0   N    -1  select         sendmail
2040   1870  1863  4515     168  0   N    -1  sigtimedwait   agent
65     38    0     4510     255* 0   CPU  0   n/a            pagezerod
43     27    0     5195     -32  1   CPU  1   n/a            progressdaemon
1592   1476  1     5195     -16  1   LDOM -1  nanosleep      midaemon
484    19    0     5194     133  1   CPU  1   n/a            ksyncer_daemon
280957 10321 10320 5190     137  1   N    -1  stat64         java
279840 10321 10320 5191     137  1   N    -1  ksleep         java
281123 25535 1871  5191     138  1   N    -1  unlink         ls
281086 25516 908   5192     148  1   N    -1  execve         inetd
2850   2548  1     4509     152  1   N    -1  ioctl          ia64_corehw
281184 25553 1384  4512     152  1   N    -1  fork           cron
281182 25551 7475  4514     152  1   N    -1  fork           nfs.mon
281183 25552 7516  4514     152  1   N    -1  fork           samba.mon
281178 10321 10320 14321923 152  1   N    -1  lwp_detached_e java
281185 10321 10320 14321923 152  1   N    -1  lwp_detached_e java
281186 10321 10320 14321923 152  1   N    -1  lwp_detached_e java
281187 10321 10320 14321923 152  1   N    -1  lwp_detached_e java
19975  10321 10320 4509     154  1   N    -1  read           java
281156 10321 10320 4509     154  1   N    -1  read           java
281157 10321 10320 4509     154  1   N    -1  read           java
274336 10321 10320 4510     154  1   N    -1  poll           java
2204   1870  1863  4512     154  1   N    -1  read           agent
682    601   1     4514     154  1   N    -1  select         syslogd
1420   1313  1     4515     154  1   N    -1  select         cimserver
1479   1365  1     4515     154  1   N    -1  poll           pwgrd
2081   1650  1     4515     154  1   N    -1  poll           vxsvc
2628   2307  1485  5188     154  1   N    -1  ksleep         rep_server
503    421   1     5189     154  1   N    -1  ksleep         utmpd
280950 10321 10320 5189     154  1   N    -1  ksleep         java
1311   1224  1     5190     154  1   N    -1  select         ipv6agt
1320   1233  1     5190     154  1   N    -1  select         mib2agt
2403   2130  1399  5192     154  1   N    -1  select         psmctd
18475  9744  7510  5192     154  1   N    -1  select         smbd
281125 10321 10320 5192     154  1   N    -1  read           java
497    421   1     5193     154  1   N    -1  ksleep         utmpd
1658   1528  1     5193     154  1   N    -1  ksleep         swagentd
2041   1871  1     5193     154  1   N    -1  select         prngd
2646   1485  1     5193     154  1   N    -1  ksleep         perflbd
2687   2379  2362  5193     154  1   N    -1  ksleep         alarmgen
13033  7508  1     5193     154  1   N    -1  select         nmbd
281001 10321 10320 5193     154  1   N    -1  poll           java
19980  10321 10320 5194     154  1   N    -1  ksleep         java
280952 10321 10320 10503    154  1   N    -1  ksleep         java
2671   2362  1485  32899    154  1   N    -1  select         agdbserver
20345  10387 10386 38407    154  1   N    -1  ksleep         java
482    417   1     5191     168  1   N    -1  sigtimedwait   syncer
1976   1650  1     5191     168  1   N    -1  nanosleep      vxsvc
2079   1650  1     5191     168  1   N    -1  nanosleep      vxsvc
2985   1303  1     5188     178  1   N    -1  sched*         dced
54     38    0     16452    255* 1   CPU  1   n/a            pagezerod


Top 10 threads sorted by %cpu
=============================
tid    pid   ticks    pri cpu state %cpu   command
===    ===   =====    === === ===== ====   =======
278662 10    5187     138 1   RUN   87.50  nfsktcpd
278663 10    5193     138 0   ZOMB  60.35  nfsktcpd
278661 10    4509     134 0   RUN   56.00  nfsktcpd
279095 10    4415     143 0   RUN   45.59  nfsktcpd
280703 10387 10502    154 0   SLEEP 13.26  java
2637   2307  5193     154 0   RUN   3.70   rep_server
3393   3053  5194     -27 1   SLEEP 3.29   cmcld
281063 10387 22119    154 0   SLEEP 1.49   java
281075 10387 22119    154 0   SLEEP 1.41   java
274134 10321 4510     154 0   SLEEP 1.32   java

==============================
= NFS Server side statistics =
==============================
Server rpc:
Connection oriented:
calls                   badcalls                nullrecv                
5167284                 0                       0                       
badlen                  xdrcall                 dupchecks               
0                       0                       78                      
dupreqs                 
0                       
Connectionless oriented:
calls                   badcalls                nullrecv                
9713                    0                       0                       
badlen                  xdrcall                 dupchecks               
0                       0                       0                       
dupreqs                 
0                       

Server nfs:
calls                   badcalls                
5176998                 0                       
Version 2: (9713 calls)
null                    getattr                 setattr                 
9713 100%               0 0%                    0 0%                    
root                    lookup                  readlink                
0 0%                    0 0%                    0 0%                    
read                    wrcache                 write                   
0 0%                    0 0%                    0 0%                    
create                  remove                  rename                  
0 0%                    0 0%                    0 0%                    
link                    symlink                 mkdir                   
0 0%                    0 0%                    0 0%                    
rmdir                   readdir                 statfs                  
0 0%                    0 0%                    0 0%                    
Version 3: (5167288 calls)
null                    getattr                 setattr                 
0 0%                    71921 1%                0 0%                    
lookup                  access                  readlink                
5072559 98%             81 0%                   0 0%                    
read                    write                   create                  
54 0%                   78 0%                   0 0%                    
mkdir                   symlink                 mknod                   
0 0%                    0 0%                    0 0%                    
remove                  rmdir                   rename                  
0 0%                    0 0%                    0 0%                    
link                    readdir                 readdir+                
0 0%                    0 0%                    21926 0%                
fsstat                  fsinfo                  pathconf                
669 0%                  0 0%                    0 0%                    
commit                  
0 0%                    
```

