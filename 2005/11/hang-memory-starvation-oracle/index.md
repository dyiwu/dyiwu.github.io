# System hang with memory starvation.

## System hang with memory starvation.
System went to memory starvation situation regarding stale shared
 memory segment does not released.

```

            System crash dump analysis report
            =================================

Symptom
-------
 System behaved as hang, INIT dump be taken for root cause finding.

Root Cause
----------
 System went to memory starvation situation regarding stale shared
 memory segment does not released (used by Oracle process), which
 leads system out of memroy so system behaved as hang.

Action Plan
-----------
 Please use ipcs -mb command to make sure there is no more stale shared
 memory in system before start up the Oracle instances.

Detail Analysis
---------------

Dump time Tue Nov 29 08:53:35 2005 UTC-8
System has been up 3 days, 21 hours, 30 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : server g4000                   
HP-UX version    : B.11.22
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : IA64 (799 Mhz)
CPU Architecture : IA-64 0
Load average     : 0.25  0.24  0.18  

			================
			= Crash Events =
			================


Crash event 0 was a INIT !
This seems to be a user initiated INIT !

			==================
			= Memory Globals =
			==================


Physical Memory     = 2096111 pages (8.00 GB)
Free Memory         = 1571 pages (6.14 MB)
Average Free Memory = 1543 pages (6.03 MB)
gpgslim             = 7182 pages (28.05 MB)
lotsfree            = 32768 pages (128.00 MB)
desfree             = 7168 pages (28.00 MB)
minfree             = 3328 pages (13.00 MB)

Free memory is desperately low (freemem lower than minfree) !
We have memory sleepers !
There are 45 deactivated processes !


maxpendpageouts = 30
pageoutrate     = 30    curr_pgrate = 36
max_pgrate      = 21   
lowmemdeact     = 35    thrashdeact = 0


			=========================
			= Physical Memory Usage =
			=========================

Kernel memory in super page pool = 12495 (48.81 MB)

Top 10 kernel memory arenas
===========================

Arena             Pages
-----             -----
VFD_BT_NODE       16353   (63.88 MB)
M_TEMP            11301   (44.14 MB)
SWAP_MISC_ARENA   6268    (24.48 MB)
spinlock          3695    (14.43 MB)
M_LVM             1114    (4.35 MB)
M_REG             822     (3.21 MB)
KMEM_ALLOC        742     (2.90 MB)
M_PREG            691     (2.70 MB)
ALLOCB_MBLK_LM    609     (2.38 MB)
VM MISC ARENA     517     (2.02 MB)

Total all arenas  47747   (186.51 MB)

			=======================
                        = Shared memory usage =
			=======================

T        ID     KEY        MODE         OWNER     GROUP        SEGSZ
Shared Memory:
m         0 0x411c0391  --rw-rw-rw-      root      root          348
m         0 0x4e0c0002  --rw-rw-rw-      root      root        61760
m         0 0x41200332  --rw-rw-rw-      root      root         8192
m    294912 0x00000000  D-rw-------      root      root      1052672
m     24576 0x00000000  D-rw-------        30     other       237576
m    499712 0x00000000  D-rw-r-----       103       103   3541905408
m  22568960 0x7cfe3530  --rw-rw----       103       103   3541905408


			=====================================
                        = Summary of processes memory usage =
			=====================================

List sorted by physical size, in pages/bytes:

                      virtual        physical            swap 
   pid   ppid   pages / bytes   pages / bytes   pages / bytes  command
  4004      1 1163380    4.4g  868152    3.3g  872691    3.3g  oracle
  4018      1 1164021    4.4g   55260  215.9m   66339  259.1m  oracle
  4056      1 1163380    4.4g   54236  211.9m   65696  256.6m  oracle
  4024      1 1163672    4.4g   54236  211.9m   66309  259.0m  oracle
  4054      1 1163031    4.4g   54236  211.9m   65657  256.5m  oracle
  3998      1 1163160    4.4g   54236  211.9m   65881  257.3m  oracle
  4044      1 1163160    4.4g   54236  211.9m   65795  257.0m  oracle
  4050      1 1163160    4.4g   54236  211.9m   65792  257.0m  oracle
  4034      1 1163160    4.4g   54236  211.9m   65795  257.0m  oracle
  4052      1 1163380    4.4g   54236  211.9m   65700  256.6m  oracle
  4040      1 1163160    4.4g   54236  211.9m   65795  257.0m  oracle
  4030      1 1163160    4.4g   54236  211.9m   65795  257.0m  oracle
  2037      1   36689  143.3m     369    1.4m     455    1.8m  sendmail
  3969   3966  278071    1.1g     288    1.1m    6297   24.6m  dbsnmp
  2270      1   35840  140.0m     180  720.0k     324    1.3m  xntpd
 17403      1   41368  161.6m     177  708.0k    2081    8.1m  mad
  3414   2497    5780   22.6m     128  512.0k    2842   11.1m  rep_server
  3423   2497    4994   19.5m     128  512.0k    2078    8.1m  agdbserver
   478      1   36398  142.2m      97  388.0k     827    3.2m  syncer
  2497      1    4572   17.9m      96  384.0k    1625    6.3m  perflbd
  3506   3423    5119   20.0m      96  384.0k    2167    8.5m  alarmgen
  2381      1   36703  143.4m      62  248.0k     590    2.3m  diagmond
  1432      1   35685  139.4m      33  132.0k     282    1.1m  ntl_reader
  1433   1432   36830  143.9m      33  132.0k     462    1.8m  netfmt
  2234      1   35744  139.6m      24   96.0k     318    1.2m  rbootd
  2924   2381   36929  144.3m      23   92.0k     662    2.6m  psmctd
  3313      1   36934  144.3m      20   80.0k     961    3.8m  ia64_corehw
  2923   2381   36347  142.0m      20   80.0k     387    1.5m  diaglogd
  3358      1   36814  143.8m      20   80.0k     544    2.1m  sysstat_em
  1422      1   35663  139.3m      19   76.0k     258    1.0m  nktl_daemon
  2768      1  267889    1.0g      18   72.0k    1867    7.3m  httpd
  1729      1   36571  142.9m      17   68.0k     438    1.7m  inetd
 19907      1   35664  139.3m      17   68.0k     262    1.0m  getty
  2314      1   36078  140.9m      17   68.0k     352    1.4m  pwgrd
  2841      1   36637  143.1m      17   68.0k     444    1.7m  kcmond
  2361      1   36118  141.1m      17   68.0k     309    1.2m  cron
  1403      1   36093  141.0m      17   68.0k     289    1.1m  ptydaemon
  3916   1729   36394  142.2m      17   68.0k     347    1.4m  telnetd
  1698      1   36218  141.5m      17   68.0k     455    1.8m  automount
  2823      1   36214  141.5m      17   68.0k     355    1.4m  p_client
  3996   3986   36084  141.0m      17   68.0k     318    1.2m  ksh
  1394      1   36063  140.9m      17   68.0k     356    1.4m  syslogd
  1659      1      20   80.0k      16   64.0k      21   84.0k  biod
  1660      1      20   80.0k      16   64.0k      21   84.0k  biod
  2518      1    3627   14.2m      16   64.0k    1530    6.0m  swagentd
  1661      1      20   80.0k      16   64.0k      21   84.0k  biod
  1668      1      20   80.0k      16   64.0k      21   84.0k  biod
  1662      1      20   80.0k      16   64.0k      21   84.0k  biod
  1667      1      20   80.0k      16   64.0k      21   84.0k  biod
  1666      1      20   80.0k      16   64.0k      21   84.0k  biod
  1663      1      20   80.0k      16   64.0k      21   84.0k  biod
  1664      1      20   80.0k      16   64.0k      21   84.0k  biod
  1665      1      20   80.0k      16   64.0k      21   84.0k  biod
  2179      1    4163   16.3m      16   64.0k    2088    8.2m  dced
  1658      1      20   80.0k      16   64.0k      21   84.0k  biod
  2117      1    2422    9.5m      16   64.0k     769    3.0m  mib2agt
  1657      1      20   80.0k      16   64.0k      21   84.0k  biod
  3997   3996      20   80.0k      16   64.0k      21   84.0k  sqlplus
  1671      1      20   80.0k      16   64.0k      21   84.0k  biod
  1672      1      20   80.0k      16   64.0k      21   84.0k  biod
  1670      1      20   80.0k      16   64.0k      21   84.0k  biod
  1669      1      20   80.0k      16   64.0k      21   84.0k  biod
  2993      1   36894  144.1m       8   32.0k     616    2.4m  disk_em
  2962      1   36922  144.2m       4   16.0k     650    2.5m  cmc_em
  3105      1   37047  144.7m       4   16.0k     783    3.1m  dm_TL_adapter
  3249      1   36999  144.5m       4   16.0k     724    2.8m  dm_stape
  2801   2768  267889    1.0g       2    8.0k    1867    7.3m  httpd
  2799   2768  267889    1.0g       2    8.0k    1867    7.3m  httpd
  2798   2768  267889    1.0g       2    8.0k    1867    7.3m  httpd
  2814      1   34993  136.7m       2    8.0k     245  980.0k  krsd
  2797   2768  267889    1.0g       2    8.0k    1867    7.3m  httpd
  2800   2768  267889    1.0g       2    8.0k    1867    7.3m  httpd
  3966      1   35849  140.0m       1    4.0k     340    1.3m  dbsnmpwd
  3986   3917   36084  141.0m       1    4.0k     318    1.2m  ksh
  2706   2701   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2713   2706   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2718   2704   36154  141.2m       1    4.0k     432    1.7m  nfsd
  1687      1   36388  142.1m       1    4.0k     619    2.4m  rpc.lockd
  1607      1   36242  141.6m       1    4.0k     530    2.1m  rpcbind
  2701      1   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2703   2701   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2687      1   36289  141.8m       1    4.0k     563    2.2m  rpc.mountd
  2454      1   36305  141.8m       1    4.0k     529    2.1m  ttd
  1681      1   36353  142.0m       1    4.0k     620    2.4m  rpc.statd
  2704   2701   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2699      1   36122  141.1m       1    4.0k     400    1.6m  nfsd
  2705   2703   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2714   2706   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2715   2703   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2720   2704   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2931   1729   36033  140.8m       1    4.0k     466    1.8m  registrar
  2712   2703   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2710   2701   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2709   2706   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2708   2701   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2717   2704   36154  141.2m       1    4.0k     432    1.7m  nfsd
  2711   2701   36154  141.2m       1    4.0k     432    1.7m  nfsd
  3917   3916   35149  137.3m       0    0.0k     292    1.1m  sh
  2132      1    2180    8.5m       0    0.0k     656    2.6m  trapdestagt
  2102      1    2184    8.5m       0    0.0k     656    2.6m  hp_unixagt
  2815      1   34964  136.6m       0    0.0k     222  888.0k  sfd
  2074      1    3931   15.4m       0    0.0k     904    3.5m  snmpdm
  2631      1    1522    5.9m       0    0.0k     529    2.1m  rstlistener
  2580      1    1698    6.6m       0    0.0k     576    2.2m  emsagent
  2856   2808    1913    7.5m       0    0.0k     607    2.4m  dtlogin
  3955      1  273848    1.0g       0    0.0k    2351    9.2m  tnslsnr
  2808      1   35149  137.3m       0    0.0k     289    1.1m  dtrc
                                     physical            swap 
                                pages / bytes   pages / bytes
                       Total: 1468167    5.6g 1661673    6.3g 

			========================
			= Buffer Cache Globals =
			========================

dbc_max_pct           = 20 %
dbc_min_pct           = 5 %
dbc current pct       = 5.0 %
bufpages              = 104804 pages (409.39 MB)
Number of buf headers = 210324

fixed_size_cache = 0 
dbc_parolemem    = 0 
dbc_stealavg     = 33554431 
dbc_ceiling      = 419222 pages (1.60 GB)
dbc_nbuf         = 52402 
dbc_bufpages     = 104805 pages (409.39 MB)
dbc_vhandcredit  = 62 
orignbuf         = 0 
origbufpages     = 0 pages

			====================
			= Swap Information =
			====================

swapinfo -mt emulation
======================

             Mb      Mb      Mb   PCT  START/      Mb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev        8192       0    8192    0%       0       -    1  LVM vg00/lv2
dev        4096     182    3914    4%       0       -    0  LVM vg00/lv10
reserve       -    7037   -7037
memory     5785    5680     105   98%
total     18073   12899    5174   71%       -       0    -

Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is CT_INIT on CPU #1;
= p crash_event_t 0xe000000100003000
= p rpb_t 0xe000000107034b40
= Process at 0xe0000001468dc000, pid 2, "vhand"
==============  EVENT  ============================
RR0=0x00ffff31  RR1=0x00ffff31  RR2=0x00ffff31  RR3=0x00ffff31
RR4=0x032ea131  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0x8003ffff7ffed6a0 0x8003ffff7ffffd90 0xe00000000034d7a0 vhand_vfdcheck_lgpg+0xc0
0x8003ffff7ffed648 0x8003ffff7ffffd90 0xe00000000034d420 vhand_vfdcheck+0x80
0x8003ffff7ffed5e8 0x8003ffff7ffffd90 0xe0000000009df470 for_val3+0xb0
0x8003ffff7ffed518 0x8003ffff7ffffd90 0xe00000000042f700 for_val2+0x2b0
0x8003ffff7ffed448 0x8003ffff7ffffdb0 0xe00000000042f7c0 for_val2+0x370
0x8003ffff7ffed370 0x8003ffff7ffffdd0 0xe00000000042f7c0 for_val2+0x370
0x8003ffff7ffed2a0 0x8003ffff7ffffdf0 0xe00000000042f7c0 for_val2+0x370
0x8003ffff7ffed258 0x8003ffff7ffffe10 0xe0000000003e32a0 foreach_valid+0x100
0x8003ffff7ffed1b8 0x8003ffff7ffffe40 0xe0000000003e1570 agepages+0x210
0x8003ffff7ffed120 0x8003ffff7ffffe80 0xe0000000004b8a70 vhand_core+0x400
0x8003ffff7ffed050 0x8003ffff7ffffea0 0xe00000000051c050 vhand+0x550
0x8003ffff7ffed020 0x8003ffff7fffff50 0xe000000000833a70 im_vhand+0x1b0
0x8003ffff7ffecf70 0x8003ffff7fffff80 0xe0000000009c4cd0 DoCalllist+0x230
------------------------------------------------------


Stack Traces for other processors 
=================================
Processor #0

==============  EVENT  ============================
= Event #2 is CT_INIT on CPU #0;
= p crash_event_t 0xe000000100003060
= p rpb_t 0xe0000001005c6a50
= Process at 0xe000000148936000, pid 2381, "diagmond"
==============  EVENT  ============================
RR0=0x0347e331  RR1=0x035ad431  RR2=0x02fe8c31  RR3=0x02fe8c31
RR4=0x035ad431  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0xe000000100006000 0xe0000001000169d0 0xe00000000044e641 idle+0x4e1
0xe000000100006000 0xe0000001000169f0 0xe000000000735920 netcall_stub
------------------------------------------------------

Processor #2

==============  EVENT  ============================
= Event #1 is CT_INIT on CPU #2;
= p crash_event_t 0xe000000100003030
= p rpb_t 0xe000000107035e00
= Process at 0xe0000001001c83c0, pid 0, "swapper"
==============  EVENT  ============================
RR0=0x00ffff31  RR1=0x00ffff31  RR2=0x00ffff31  RR3=0x00ffff31
RR4=0x019e6331  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0xe000000100d84000 0xe000000100d949d0 0xe00000000044e5a1 idle+0x441
0xe000000100d84000 0xe000000100d949f0 0xe000000000735920 netcall_stub
------------------------------------------------------

Processor #3

==============  EVENT  ============================
= Event #3 is CT_INIT on CPU #3;
= p crash_event_t 0xe000000100003090
= p rpb_t 0xe0000001070370c0
= Process at 0xe0000001468e2000, pid 3, "statdaemon"
==============  EVENT  ============================
RR0=0x00ffff31  RR1=0x00ffff31  RR2=0x00ffff31  RR3=0x00ffff31
RR4=0x01eb1431  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0xe000000100d9a000 0xe000000100daa9d0 0xe00000000044e5b1 idle+0x451
0xe000000100d9a000 0xe000000100daa9f0 0xe000000000735920 netcall_stub
------------------------------------------------------

			=================
			= Load Averages =
			=================


avenrun
=======
0.25  0.24  0.18  

mp_avenrun
==========
cpu0 : 0.010690  0.258792  0.228665  
cpu1 : 0.429470  0.119369  0.063108  
cpu2 : 0.552727  0.581258  0.383355  
cpu3 : 0.001580  0.021192  0.029865  

			======================
			= Thread Information =
			======================

32 Threads ran in the last second
42 Threads ran in the last 5 seconds
50 Threads ran in the last 10 seconds
50 Threads ran in the last minute
161 Threads ran in the last hour

statdaemon ran 20 ticks ago


Most Common Wait Channels
=========================
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
memory_sleepers                                    49     81143      24520     
vx_worklist_thread_sv                              28     246        46        
async_bufhead                                      16     33632440   33632439  
per_processor_selects+0xc0                         7      110796     94511     
per_processor_selects+0x40                         7      110796     29390     
per_processor_selects                              7      110796     3         
lvmkd_q                                            6      521        521       
schedcpu_procp                                     6      274        74        
per_processor_selects+0x80                         5      110796     94512     
0xe0000001406e2080                                 4      33627541   33627540  
0xe0000001406e2040                                 4      33627541   33627541  
0xe0000001406e2100                                 4      33627540   33627539  
0xe0000001406e20c0                                 4      33627540   33627540  
streams_mp_sync                                    4      80376      27517     
ken_recv_msg(0xe0000001469de040)                   3      33658656   33658656  
0xe000000149de5430                                 3      85029      84780     
streams_blk_sync                                   2      33658656   33658656  
sigsuspend(0x8003ffff7ffec000)                     2      26714      93        
ticks_since_boot                                   2      20         20        


Most Common Sleep Callers
=========================
                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
reserve_freemem()                                  49     81143      24520     
vx_worklist_thread()                               28     246        46        
ksleep()                                           27     33656715   81773     
async_daemon()                                     16     33632440   33632439  
select()                                           8      110796     3         
lvmkd_daemon()                                     6      521        521       
pm_schedcpu()                                      6      274        74        
sosleep()                                          4      3470634    81775     
ken_recv_msg()                                     3      33658656   33658656  
pm_sigwait()                                       2      33628219   48        
streams_getmsg()                                   2      110796     110182    
sigsuspend()                                       2      26714      93        

```

