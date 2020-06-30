# System mini hang when long run queue.

## System mini hang when long run queue.
Midaemon daemon process was spining in user space, which
monopolized the SPU, leads system mini-hang.
  
```

System Crash Dump Analysis Report
=================================

Symptom
-------
  Long run queue length. System be TOC for crashdump analysis.

Root Cause
----------
  Midaemon daemon process was spining in user space, which
  monopolized the SPU, leads system mini-hang.

Action Plan
-----------
 Upgrade the GlancePlusePak from C.02.40 to C.03.72 on Dart 63 Dec/2003

Delay Analysis
--------------

Dump time Mon Apr  5 10:18:00 2004 UTC-8
System has been up 132 days, 18 hours, 11 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/N4000-36
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : PCXW (360 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 264.68  263.79  258.57  

			================
			= Crash Events =
			================

==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x95ab58
= Using pc from pim.wide.rp_pcoq_head_hi = 0x34548
==============  EVENT  ============================
SR5=0x07703400
                SP         RP Return Name
0x400003ffffff0da0 0x00034548 setjmp+0x3c
0x400003ffffff0da0 0x0019b9d4 syscall+0x44c
0x400003ffffff0c70 0x00035944 syscallinit+0x54c


Stack Traces for other processors 
=================================


Processor #1
==============  EVENT  ============================
= Event #1 is TOC on CPU #1
= p crash_event_t 0x22030
= p rpb_t 0xf31730
= Using pc from pim.wide.rp_pcoq_head_hi = 0x4233b
= USER mode!
==============  EVENT  ============================

		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 2


        s
        t
        a       spin reg eiem/spl         eirr             ipsw            
evt cpu t type  dpth src cr15             cr23             cr22            
--- --- - ----- ---- --- ---------------- ---------------- ----------------
0   0   E TOC   0    rpb fffffff0ffffffff 0000000000000000 0804fc1f WCRQPDI
1   1   E TOC   0    rpb ffffffffffffffff 0000000000000000 0004001f CRQPDI

SPL/EIEM values:

0xfffffffeffffffff = SPLPREEMPTOK - Default user mode SPL level.
0xfffffff0ffffffff = SPLNOPREEMPT - Disable kernel preemption (scheduling interrupt off).
0xffffff00ffffffff = SPL2 - Disable software interrupt (software triggers off).
0xef00080000000000 = SPL5 - Disable IO modules.
0xc700000000000000 = SPL6+CLOCK_RESYNC - Disable hardclock+enable clock-resync.
0xc600000000000000 = SPL6 - Disable hardclock.
0x0000000700000000 = SPL7/PSW_I=0 - Disable the world.


			========================
			= Processor Clock Info =
			========================

hardclock_late = 0
itick_per_tick = 3600000
lbolt          = 1147033108 (0x445e5614)

    event mpi                rpb                delta      clk eiem eirr PSW
cpu type  timeinval          interval timer     secs:ticks od  0,4  0,4  I
--- ----- ------------------ ------------------ ---------- --- ---- ---- ---
0   TOC   0xeaba74929e173    0xeaba74911a714         0:0   0   1 1  0 0  1 
1   TOC   0xeaba7491d57fa    0xeaba748f6d3a8         0:0   0   1 1  0 0  1 

			=================
			= Load Averages =
			=================


avenrun
=======
264.68  263.79  258.57  

real_run
========
529.361571  527.573791  517.119005  

pwrun ("fast" io wait)
======================
0.003111  0.012762  0.014265  

mp_avenrun
==========
cpu0 : 264.776373  263.932132  258.755735  
cpu1 : 264.588309  263.654421  258.377536  

			======================
			= Thread Information =
			======================

46 Threads ran in the last second
148 Threads ran in the last 5 seconds
275 Threads ran in the last 10 seconds
468 Threads ran in the last minute
637 Threads ran in the last hour

statdaemon ran 43 ticks ago


Most Common Wait Channels
=========================
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
selwait                                            49     1147004892 2         
vx_inactive_thread_sv                              25     41331      1319      
vx_inactive_thread_sv+0x8                          25     37731      1725      
async_bufhead                                      16     16889106   16889105  
0x100006740                                        10     45865      45860     
0x100006700                                        9      11501      10520     
*uptr+0                                            7      242280229  39        
lvmkd_q                                            6      230        230       
svc_run(0x100006700)                               3      11502      11499     
streams_blk_sync                                   2      1147028279 1147028279
svc_run(0x100006740)                               2      45862      11498     
locklist+0x1f48                                    2      237        237       
streams_mp_sync                                    2      143        43        


Most Common Sleep Callers
=========================
                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
wait1()                                            82     1146931784 1504      
fifo_rdwr()                                        34     14432612   1336      
svc_run()                                          17     45865      10521     
select()                                           15     1146928820 338       
pm_sigwait()                                       5      27438      72        
hpstreams_read_tty()                               3      45505      23431     
poll()                                             2      274729458  31        
sigsuspend()                                       2      12576756   10896364  
locked()                                           2      237        237       


Idle Globals
============

candidate_idle_spu = -1
migration_cycles = 0

Running Threads (TSRUNPROC) and idle Processors
===============================================


                    TICKS      TICKS                    I TICKS
                    SINCE      SINCE                    C SINCE      NREADY
TID     PID   PPID  RUN        IDLE       PRI SPU STATE S MIGR       FR LO AL COMMAND
------- ----- ----- ---------- ---------- --- --- ----- - ---------  -- -- -- -------
27710   26610 1223  0          24869664   178 0   SYS   N 24869664   264 0  0  registrar 
21221   12527 1     0          22494359   -16 1   USER  N 22494359   266 0  0  midaemon 

Note: 
FR: free to run on any processor (candidate for thread migration).
LO: locked (via processor affinity/mpctl) to this processor).
AL: Alpha semaphores misses (special scheduling when miss a sema).


   pid     tid pri spu kt_cpu  recent      user       sys     intr    kt_start p_comm
 12527   21221 -16   1   255        8   5655996    103366        0  0x4046d62f midaemon
 11922   13377 180   1     8        3     67887    126051        0  0x406d8355 registrar
  1002    2141 180   0     8        7     10818     26657        0  0x406fbedc registrar
  4778    5866 180   0     8        8     10940     20689        0  0x406fddcf registrar
  8613    9678 180   0     8        8      8837     17522        0  0x406ffcc2 registrar
  1411    2579 184   0     8        0         4         4        0  0x406ed1b5 ps
 27091   28191 180   0     8        6         0         0        0  0x406fa58a registrar
 21679   22776 180   1     8       13         0         0        0  0x406f8f08 registrar
  6225    7821 179   1     7        0     76042    181285        0  0x406d3cff registrar
 28248   29697 179   0     7        0     65752    122870        0  0x406d88f5 registrar
 23450   24839 179   1     7        0     59109    110641        0  0x406dad89 registrar
 13132   14507 179   0     7        0     57842    108165        0  0x406db5fa registrar
 21450   22788 179   1     7        0     53424     99649        0  0x406dd7be registrar
 20812   22077 179   0     7        0     45271     84765        0  0x406e1874 registrar
 25718   26987 179   0     7        0     44547     83908        0  0x406e1b44 registrar
  7479    8699 179   1     7        0     41447     77579        0  0x406e42a7 registrar
 11604   12788 179   1     7        0     37479     70449        0  0x406e6a0b registrar
 10220   11328 179   0     7        0     17990     33946        0  0x406f64cd registrar
 24681   25779 179   1     7        0     14704     27754        0  0x406f9a49 registrar
  5843    6923 179   0     7        0     10434     19707        0  0x406fe640 registrar
 10468   11536 179   0     7        0      8705     16595        0  0x40700544 registrar
 17194   18247 179   0     7        0      6754     12742        0  0x40702cc0 registrar
  1810    2849 179   0     7        0      4603      8686        0  0x407059fb registrar
 13634   14694 179   0     7        0         0         0        0  0x4070163f registrar
 12380   13383 179   0     7        0         0         0        0  0x4070bf73 nfsmc
  4061    5428 179   0     7        0         0         0        0  0x406dc13b registrar
  8478   10209 179   0     6        0    121053    219754        0  0x406d0789 registrar
 20719   22147 179   0     6        0     64051    119235        0  0x406d9166 registrar
 15041   16378 179   0     6        0     53152     99117        0  0x406dd4ed registrar
   983    2371 179   0     6        0     51646     96785        0  0x406ddd5e registrar
 12145   13473 179   0     6        0     51325     96226        0  0x406de2ff registrar
 15523   16787 179   1     6       12     45065     84914        0  0x406e15a3 registrar
 21926   23174 179   1     6        0     43218     80831        0  0x406e2c25 registrar
  2752    3987 179   1     6        0     39863     75231        0  0x406e50b9 registrar
 26702   27883 179   1     6        0     36193     68532        0  0x406e754c registrar
  5168    6328 179   0     6        0     33169     62915        0  0x406e9f80 registrar
 26282   27426 179   0     6        0     30070     56848        0  0x406ec422 registrar
 22342   23478 179   0     6        0     29838     56561        0  0x406ebe73 registrar
 18598   19718 179   0     6        0     26251     49475        0  0x406eebaf registrar
 22498   23616 179   0     6        0     25706     48783        0  0x406ef70b registrar
  2695    3837 179   0     6        0     14399     47190        0  0x406f321f registrar
 29726     884 179   1     6        0      5019      9596        0  0x40704eac registrar
 20713   21810 179   1     6        0         0         0        0  0x406f8c37 registrar
 12858   13982 179   0     6        0         0         0        0  0x406ee04e registrar
  5597    6746 179   0     6        0         0         0        0  0x406ea250 registrar
  4049    5200 179   1     6        0         0         0        0  0x406e9cb0 registrar
 18425   19708 179   1     5        0     46935     88028        0  0x406e0792 registrar
 24332   25617 179   0     5        0     46542     87062        0  0x406e0a62 registrar
  3632    4891 179   1     5        0     44952     84090        0  0x406e20e4 registrar
 11261   12500 179   0     5        0     42294     79773        0  0x406e3496 registrar
  1283    2541 179   0     5        0     41379     78037        0  0x406e3fd7 registrar
  2184    3346 179   1     5        0     28597     53902        0  0x406ed233 registrar
 11532   12661 179   0     5        0     27924     53022        0  0x406edd7e registrar
 23477   24594 179   1     5        0     25423     47968        0  0x406efcab registrar
 26251   27377 179   1     5        0     24680     46785        0  0x406f07ec registrar
  4845    5970 179   0     5        0     20859     39565        0  0x406f3d60 registrar
  5672    6785 179   0     5        0     20591     38887        0  0x406f4300 registrar
 22414   23507 179   0     5        5     15252     28796        0  0x406f91d8 registrar
  1533    2638 179   1     5        0     12434     23530        0  0x406fc47d registrar
 14648   15702 179   1     5        0      7598     14409        0  0x40701bdf registrar
 25146   26192 179   0     5        0      5820     11053        0  0x40703dba registrar
  3346    4372 179   1     5        0      4072      7759        0  0x4070653b registrar
 28501   29625 183   0     5        1         0         0        0  0x406f1b93 find
 28107   29155 179   0     5        0         0         0        0  0x40704906 registrar
 26707   27832 179   0     5        0         0         0        0  0x406f0d8c registrar
 20667   21860 179   0     5        0         0         0        0  0x406e5bfa registrar
  4448    5572 179   0     5        0         0         0        0  0x406f3a90 registrar
  3292    4810 179   0     5        0         0         0        0  0x406d6193 registrar
   886    2055 179   0     5        0         0         0        0  0x406f26de registrar
  7592    9027 179   0     4        0     65566    121695        0  0x406d8bc6 registrar
  3261    4556 179   0     4        0     47450     89151        0  0x406dff21 registrar
   163    1395 179   1     4        0     38411     72652        0  0x406e619a registrar
 21057   22269 179   1     4        8     46952     71962        0  0x406e4b18 registrar
 14472   15592 179   0     4        0     27224     51262        0  0x406ee324 registrar
 20840   21960 179   0     4        0     26050     49194        0  0x406ef16a registrar
 26513   27639 179   0     4        0     24404     46351        0  0x406f0abc registrar
 27235   28361 179   0     4        0     24151     45550        0  0x406f105d registrar
  7075    8184 179   0     4        0     18882     39839        0  0x406f4b71 registrar
 13151   14253 179   0     4        0     17336     32643        0  0x406f75af registrar
  4042    5124 179   0     4        0     11291     21306        0  0x406fd82f registrar
  7030    8103 179   1     4        0      9929     18827        0  0x406feeb1 registrar
 12745   13808 179   1     4        0      7983     15139        0  0x4070136f registrar
   495    1570 179   0     4        0      5004      9468        0  0x4070517c registrar
 25610   26711 179   1     4        0         0         0        0  0x406f9e1b gzip
 19475   20631 179   1     4        0         0         0        0  0x406e8bce registrar
 13123   14123 179   0     4        0         0         0        0  0x4070c192 getCwbGridKey
  9867   10875 179   0     4        0         0         0        0  0x4070aba9 registrar
  5221    6253 179   1     4        0         0         0        0  0x4070815d registrar
 27296   29084 179   0     3        0    143585    259216        0  0x406cf977 registrar
  8226    9839 179   0     3        0     91733    169903        0  0x406d348f registrar
 23293   24855 179   0     3        0     83535    155264        0  0x406d4b10 registrar
 19568   21110 179   0     3        0     80068    148855        0  0x406d5381 registrar
  8016    9306 179   0     3        0     47771     89120        0  0x406e01f1 registrar
 16319   17549 179   0     3        0     42447     80102        0  0x406e3766 registrar
 24347   25469 179   0     3        0     24942     47449        0  0x406eff7b registrar
 26012   27136 179   0     3        0     24799     46832        0  0x406f051c registrar
 27768   28893 179   0     3        0     23605     44489        0  0x406f15fd registrar
 29527     765 179   1     3        0     22840     43172        0  0x406f1e6e registrar
 11025   12134 179   1     3        0     17962     34193        0  0x406f6a6d registrar
 26610   27710 178   0     3        3     13990     26602        0  0x406fa2ba registrar
  6686    7755 179   1     3        0      6962     22850        0  0x406febe1 registrar
  2180    3286 179   0     3        0     12012     22805        0  0x406fca1d registrar
 16300   17354 179   1     3        0      6909     13009        0  0x407029f0 registrar
  4188    5208 179   0     3        0      3591      6808        0  0x4070707c registrar
 13150   14149 154   0     3        3        25         6        0  0x4070c1aa nfshc
 27817   29366 179   0     3        0         0         0        0  0x406d5652 registrar
 21540   22587 179   1     3        0         0         0        0  0x4070353e registrar
 12825   13930 179   0     3        0         0         0        0  0x406f72de registrar
     3       3 128   1     3        0         0         0        0  0x3fc1bbea statdaemon
  2251    3939 179   0     2        0     98856    182793        0  0x406d23ac registrar
 11513   13028 179   0     2        0     75336    139988        0  0x406d6463 registrar
 13659   15069 179   1     2        0     59287    123577        0  0x406d99d7 registrar
  5471    6874 179   1     2        0     61122    113499        0  0x406da518 registrar
 27584   28963 179   1     2        0     56606    105799        0  0x406dbe6a registrar
 25807   27168 179   0     2        0     54570    102372        0  0x406dcc7c registrar
 11834   13055 179   0     2        0     43409     74556        0  0x406e4577 registrar
 16394   17585 179   0     2        0     39181     73576        0  0x406e592a registrar
 15001   16102 179   1     2        0     16139     30394        0  0x406f80ef registrar
 28304   29400 179   0     2        0     12730     24230        0  0x406fadfb registrar
 15886   16939 179   0     2        0      6945     13053        0  0x40702720 registrar
 19894   20944 179   1     2        0      6653     12534        0  0x40703268 registrar
 22672   23720 179   1     2        0      6098     11572        0  0x4070381a registrar
   988    2072 179   1     2        0      4765      9048        0  0x4070545a registrar
  3947    4972 179   1     2        0      3756      7076        0  0x40706dac registrar
 13124   14124 179   0     2        0        31         2        0  0x4070c192 decodeSynop
 13205   14204 178   0     2        1         0         1        0  0x4070c1cc lzo
 28485   29609 182   0     2        1         0         0        0  0x406f1b8b rm
 12370   13372 179   0     2        0         0         0        0  0x4070bf6d registrar
  9629   10700 179   0     2        0         0         0        0  0x40700266 registrar
  6430    7559 182   0     2        0         0         0        0  0x406ed690 basename
  5856    6876 179   0     2        0         0         0        0  0x407089cd registrar
 23194   24917 179   1     1        0    119000    216684        0  0x406d0a5a registrar
 27172   28763 179   0     1        0     85530    157761        0  0x406d42a0 registrar
  8484    9956 179   0     1        0     68680    128193        0  0x406d7814 registrar
 16344   17663 179   1     1        0     51765     96336        0  0x406de5cf registrar
 25956   27151 179   0     1        0     38503     72339        0  0x406e5eca registrar
 16314   17489 179   0     1        0     37288     70651        0  0x406e6cdb registrar
  8287    9450 179   0     1        0     36166     67932        0  0x406e808d registrar
 10946   12111 179   0     1        0     35756     67229        0  0x406e835d registrar
  6649    7798 179   0     1        0     37701     58172        0  0x406ea520 registrar
 27346   28491 179   1     1        0     29629     55879        0  0x406ec9c2 registrar
 27841   28983 178   1     1        0     28804     54673        0  0x406ecc92 registrar
   440    1604 179   1     1        0     21944     44680        0  0x406f240e registrar
  6683    7791 179   1     1        0     20175     37886        0  0x406f48a1 registrar
 19046   20145 179   0     1        0     15403     29292        0  0x406f8960 registrar
  1871    2962 179   0     1        0     12127     22841        0  0x406fc74d registrar
 12070   13139 179   0     1        0      8132     15365        0  0x4070109e registrar
 23763   24809 179   0     1        0      5980     11371        0  0x40703aea registrar
  5338    6360 179   0     1        0      2631      5030        0  0x4070842d registrar
  7903    8915 179   1     1        0      1666      3146        0  0x40709ac8 registrar
  8497    9503 179   1     1        0      1478      2822        0  0x40709d98 registrar
  1865    3009 149   0     1        1         4         4        0  0x406ed1f4 ps
 13215   14214 178   0     1        0         0         0        0  0x4070c1d3 date
 13211   14210 178   0     1        0         0         0        0  0x4070c1d0 date
 13206   14205 178   1     1        0         0         0        0  0x4070c1cd date
 13203   14202 178   0     1        0         0         0        0  0x4070c1ca wc
 13183   14181 178   0     1        0         0         0        0  0x4070c1bc grep
 13158   14156 178   0     1        1         0         0        0  0x4070c1ad awk
  6809    8137 179   0     1        0         0         0        0  0x406de02f registrar
  6252    7380 182   0     1        0         0         0        0  0x406ed5dd rm
  5087    6121 179   1     1        0         0         0        0  0x40707e8d registrar
  2029    3165 182   0     1        0         0         0        0  0x406ed214 wc
  6767    9757 179   0     0        0    968399   3309559     2742  0x406bcd4a dm_ses_enclosu
 12527   21218 -16   0     0        4     39809   2535013    60536  0x4046d62e midaemon
 19604   23928 178   0     0        0  12806621   2005128    17354  0x404bc9be rpc.ldmd
   330     387 154   1     0        0       593   1167422     9954  0x3fc1bc01 syncer
   572     629 154   0     0        0    201480   1035173      764  0x3fc1bc25 ilmid
  1996    2054 154   0     0        0    146067    454762      354  0x3fc1bd2f psmctd
  1994    2052 158   0     0        0     78382    424460      622  0x3fc1bd2f diaglogd
  3170    3228 154   0     0        0         0    342281    62003  0x3fc1bf70 nfsd
  3154    3212 154   0     0        0         0    333736    61492  0x3fc1bf70 nfsd
  3134    3192 154   0     0        0         1    332809    62029  0x3fc1bf70 nfsd
  3164    3222 154   0     0        0         0    331675    61339  0x3fc1bf70 nfsd
  3157    3215 154   0     0        0         0    330536    61800  0x3fc1bf70 nfsd
  3167    3225 154   0     0        0         0    327509    62144  0x3fc1bf70 nfsd
 26097   27907 179   1     0        0    153304    273856        0  0x406cf337 registrar
 15101   16883 179   0     0        0    145209    263673        0  0x406cf6a7 registrar
 28116   29883 179   1     0        0    126688    253394        0  0x406cff18 registrar
 14172   15935 179   0     0        0    138854    251851        0  0x406cfc48 registrar
 13816   15560 179   0     0        0    131634    240650        0  0x406d01e9 registrar
  5901    7615 179   0     0        0    118085    216436        0  0x406d0d2a registrar
 16830   18532 179   1     0        0    116232    212708        0  0x406d0ffa registrar
  3152    3210 154   1     0        0         0    209851    48619  0x3fc1bf70 nfsd
  3162    3220 154   1     0        0         0    209308    48859  0x3fc1bf70 nfsd
  3165    3223 154   1     0        0         0    208284    48857  0x3fc1bf70 nfsd
  3171    3229 154   1     0        0         0    208145    49079  0x3fc1bf70 nfsd
  3172    3230 154   1     0        0         0    206165    48979  0x3fc1bf70 nfsd
  3159    3217 154   1     0        0         0    205884    48792  0x3fc1bf70 nfsd
   481    2245 179   1     0        0    111556    205499        0  0x406d12ca registrar
  3169    3227 154   1     0        0         0    204797    48791  0x3fc1bf70 nfsd
 24247   25933 179   1     0        0    102604    204702        0  0x406d186b registrar
  3156    3214 154   1     0        0         0    204286    49123  0x3fc1bf70 nfsd
  3160    3218 154   1     0        0         0    203582    48874  0x3fc1bf70 nfsd
  3155    3213 154   1     0        0         0    201837    49158  0x3fc1bf70 nfsd
  3016    4719 179   0     0        0    105413    194474        0  0x406d1b3b registrar
 24640   26307 179   1     0        0    102432    189492        0  0x406d20dc registrar
 15506   17170 179   0     0        0    102563    189425        0  0x406d1e0c registrar
 16355   17999 179   1     0        0     98944    182052        0  0x406d267d registrar
 23636   25282 179   1     0        0     96775    178535        0  0x406d294d registrar
 15913   17537 179   1     0        0     96227    177366        0  0x406d2eee registrar
  5086    6734 179   1     0        0     95719    177279        0  0x406d2c1d registrar
 24866   26496 179   1     0        0     92843    171520        0  0x406d31be registrar
   513     570 127   1     0        0     45810    169476    26084  0x3fc1bc20 netfmt
 21934   26258 168   0     0        0   2187851    165483      170  0x404bca51 NpqFwd
 26116   27725 179   1     0        0     88777    164643        0  0x406d3a2f registrar
 14862   16448 179   0     0        0     87990    162951        0  0x406d3fd0 registrar
  5963    7538 179   0     0        0     85328    157168        0  0x406d4570 registrar
  1993    2051 158   0     0        0     81199    157058     8042  0x3fc1bd2f cclogd
 22807   24330 179   0     0        0     61290    153130        0  0x406d5ec2 registrar
 29973    1687 179   1     0        0     81698    151542        0  0x406d4de1 registrar
 10861   12416 179   0     0        0     81888    151132        0  0x406d50b1 registrar
 26970   28441 179   1     0        0     54331    146132        0  0x406d7db5 registrar
  5167    6714 179   0     0        0     77869    144894        0  0x406d5922 registrar
 12996   14525 179   0     0        0     77128    142789        0  0x406d5bf2 registrar
 17917   19419 179   1     0        0     74035    138160        0  0x406d6733 registrar
 25830   27340 179   1     0        0     73618    136939        0  0x406d6a03 registrar
 15056   16542 179   1     0        0     73385    134966        0  0x406d6fa4 registrar
  6152    7648 179   1     0        0     72137    134548        0  0x406d6cd3 registrar
 23241   24723 179   1     0        0     71037    132083        0  0x406d7274 registrar
 19739   21181 179   0     0        0     66964    124169        0  0x406d8625 registrar
 14374   15801 179   1     0        0     64949    121354        0  0x406d8e96 registrar
 18262   19653 179   0     0        0     52624    120365        0  0x406daab8 registrar
 27404   28838 179   0     0        0     64146    119105        0  0x406d9436 registrar
 25026   26439 179   1     0        0     60025    118719        0  0x406d9f77 registrar
  5877    7297 179   0     0        0     62964    117659        0  0x406d9706 registrar
 19356   20766 179   1     0        0     62794    116456        0  0x406d9ca7 registrar
 29751    1300 179   1     0        0     60961    114026        0  0x406da247 registrar
 12290   13690 179   0     0        0     59542    111104        0  0x406da7e8 registrar
  5848    7232 179   0     0        0     59027    109890        0  0x406db329 registrar
 29738    1268 179   1     0        0     58600    108864        0  0x406db059 registrar
 25765   27108 179   1     0        0     43241    108622        0  0x406dda8e registrar
 18547   19920 179   0     0        0     56622    106176        0  0x406db8ca registrar
 23560   24933 179   0     0        0     56998    105848        0  0x406dbb9a registrar
 10174   11537 179   1     0        0     56751    105457        0  0x406dc40b registrar
 15216   16571 179   1     0        0     55747    103734        0  0x406dc6db registrar
 20189   21546 179   1     0        0     54966    102953        0  0x406dc9ab registrar
  1768    3130 179   1     0        0     53955    101085        0  0x406dcf4c registrar
  8345    9688 179   1     0        0     53968    100375        0  0x406dd21d registrar
 27810   29135 179   1     0        0     50550     94821        0  0x406deb70 registrar
 21298   22618 179   0     0        0     50519     94696        0  0x406de89f registrar
  3715    5028 179   1     0        0     50247     93711        0  0x406dee40 registrar
  4929    6217 179   1     0        0     37276     93542        0  0x406e1003 registrar
  8372    9675 179   1     0        0     49562     92480        0  0x406df110 registrar
 11629   12939 179   1     0        0     49110     91477        0  0x406df3e0 registrar
 16689   17988 178   1     0        0     49011     91431        0  0x406df6b1 registrar
 21687   22989 179   1     0        0     48305     90726        0  0x406df981 registrar
 27546   28853 179   1     0        0     48043     90093        0  0x406dfc51 registrar
 12493   13783 179   1     0        0     46826     87264        0  0x406e04c2 registrar
  8618    9889 178   0     0        0     46203     86146        0  0x406e12d3 registrar
  9242   10497 179   1     0        0     44458     83601        0  0x406e23b5 registrar
 29665    1066 179   1     0        0     50969     83298        0  0x406e0d32 registrar
 14496   15742 179   1     0        0     43560     81630        0  0x406e2685 registrar
 29951    1356 179   0     0        0     47683     80417        0  0x406e1e14 registrar
 24815   26046 179   0     0        0     41850     78397        0  0x406e3d07 registrar
  7958    9157 178   1     0        0     38702     77613        0  0x406e5389 registrar
 17222   18431 179   1     0        0     40890     76699        0  0x406e4848 registrar
 26679   27897 179   1     0        0     40493     75934        0  0x406e4de8 registrar
  4216    5457 179   1     0        0     49206     75831        0  0x406e31c6 registrar
 11365   12567 179   1     0        0     39407     74080        0  0x406e5659 registrar
  4313    5501 179   1     0        0     38137     71595        0  0x406e646b registrar
  7535    8718 179   0     0        0     37383     70733        0  0x406e673b registrar
 25361   26525 179   0     0        0     28676     70708        0  0x406e943f registrar
  3039    4235 179   1     0        0     36604     68830        0  0x406e7aec registrar
 23268   24441 179   0     0        0     36546     68694        0  0x406e727c registrar
  5390    6557 179   1     0        0     35722     67671        0  0x406e7dbd registrar
 16806   17960 179   0     0        0     34988     66321        0  0x406e88fe registrar
 13843   14999 179   1     0        0     34983     66269        0  0x406e862d registrar
 21508   22665 179   0     0        0     34696     65766        0  0x406e8e9e registrar
 13261   21953 154   1     0        0    122552     65485        6  0x4046d653 alarmgen
 28432   29591 179   0     0        0     33707     63506        0  0x406e970f registrar
  1056    2263 179   0     0        0     33446     63075        0  0x406e99df registrar
  8669    9812 179   1     0        0     32360     60933        0  0x406ea7f1 registrar
 15817   16954 179   0     0        0     32131     60477        0  0x406eb062 registrar
 11203   12351 179   0     0        0     31873     60352        0  0x406eaac1 registrar
 17712   18849 179   1     0        0     31975     59921        0  0x406eb332 registrar
 18165   19302 179   1     0        0     31001     58803        0  0x406eb602 registrar
  8203    9331 179   1     0        0     26146     56654        0  0x406ed7de registrar
 26827   27974 179   1     0        0     29273     55626        0  0x406ec6f2 registrar
 29288     534 179   1     0        0     28630     54330        0  0x406ecf62 registrar
  9699   10828 179   1     0        0     28029     53172        0  0x406edaae registrar
 15746   16864 179   1     0        0     29377     50684        0  0x406ee5fa registrar
 16978   18097 179   0     0        0     26519     50384        0  0x406ee8d0 registrar
 20637   24961 168   1     0        0    315256     49977     2013  0x404bca29 NotifyLog
 20063   21182 179   1     0        0     26184     49593        0  0x406eee95 registrar
 21536   22655 179   0     0        0     25791     48983        0  0x406ef43b registrar
 23047   24167 179   1     0        0     25352     48147        0  0x406ef9db registrar
 12638   21330 127   1     0        0    662861     46982     7884  0x4046d631 scopeux
 29925    1162 179   1     0        0     18411     46966        0  0x406f213e registrar
 28208   29332 179   1     0        0     22420     45550        0  0x406f18cd registrar
 28508   29632 179   1     0        0     23010     45137        0  0x406f1b9e registrar
  2342    3486 179   1     0        0     21871     41272        0  0x406f2f4f registrar
  3369    4481 179   0     0        0     21512     40580        0  0x406f34ef registrar
  3667    4780 179   1     0        0     20979     39900        0  0x406f37c0 registrar
  5220    6344 179   1     0        0     20590     38844        0  0x406f4030 registrar
  6046    7155 179   0     0        0     20113     37870        0  0x406f45d1 registrar
  8009    9116 179   0     0        0     19491     36762        0  0x406f5112 registrar
  7673    8780 179   0     0        0     19306     36643        0  0x406f4e41 registrar
 10545   11656 179   0     0        0     16383     36424        0  0x406f679d registrar
  8354    9460 179   0     0        0     19100     36074        0  0x406f53e2 registrar
 11944   13053 179   1     0        0     15173     35844        0  0x406f6d3e registrar
  8725    9830 179   1     0        0     18995     35812        0  0x406f56bc registrar
 13047   21739 154   0     0        0     62604     35335       31  0x4046d64d rep_server
  8942   10049 179   0     0        0     18607     35125        0  0x406f598c registrar
  9802   10911 179   0     0        0     17785     34680        0  0x406f61fd registrar
  9314   10422 179   1     0        0     21579     33425        0  0x406f5c5c registrar
 14360   15460 179   0     0        0     16070     32860        0  0x406f7e1f registrar
 12278   13386 179   1     0        0     17123     32497        0  0x406f700e registrar
 13951   15051 179   0     0        0     16678     31622        0  0x406f7b4f registrar
 16451   17546 179   1     0        0     16009     30368        0  0x406f83c0 registrar
 17699   18789 179   1     0        0     16056     30314        0  0x406f8690 registrar
 23335   24423 179   1     0        0     15392     29080        0  0x406f94a9 registrar
 27952   29048 179   1     0        0     11198     28400        0  0x406fab2b registrar
 23849   24945 179   0     0        0     14767     28170        0  0x406f9779 registrar
 26019   27116 179   1     0        0     14572     27741        0  0x406f9fea registrar
 25415   26515 179   1     0        0     14405     27391        0  0x406f9d1a registrar
 13235   21927 154   0     0        0     39006     26893       30  0x4046d652 agdbserver
 27601   28695 179   0     0        0     13651     25969        0  0x406fa85b registrar
 28584   29680 179   0     0        0     13485     25202        0  0x406fb0cb registrar
 29045     243 179   1     0        0     13255     25181        0  0x406fb39c registrar
 29576     785 179   1     0        0     13119     24911        0  0x406fb66c registrar
   701    1825 179   0     0        0     12889     24529        0  0x406fbc0c registrar
  1282    2402 179   0     0        0     12361     23475        0  0x406fc1ac registrar
  2528    3639 179   0     0        0     11827     22310        0  0x406fcced registrar
  2778    3885 179   1     0        0     11727     22290        0  0x406fcfbe registrar
  3269    4351 179   0     0        0     11410     21684        0  0x406fd28e registrar
  4405    5485 179   0     0        0     11030     20813        0  0x406fdaff registrar
  5561    6636 179   1     0        0     10612     20091        0  0x406fe370 registrar
  6230    7302 179   1     0        0     10185     19269        0  0x406fe910 registrar
  7460    8534 179   1     0        0     10075     19013        0  0x406ff181 registrar
  7801    8873 179   0     0        0      9711     18435        0  0x406ff451 registrar
  8090    9161 179   0     0        0      9522     17949        0  0x406ff722 registrar
  8305    9375 179   1     0        0      9325     17714        0  0x406ff9f2 registrar
  9031   10100 179   0     0        0      9050     17167        0  0x406fff96 registrar
 10879   11947 178   1     0        0      8682     16386        0  0x4070081c registrar
 11298   12367 179   0     0        0      8507     16019        0  0x40700afe registrar
 14092   15151 179   0     0        0      7639     14531        0  0x4070190f registrar
 15058   16113 179   0     0        0      7543     14266        0  0x40701eaf registrar
 15377   16431 179   1     0        0      7209     13688        0  0x4070217f registrar
 15675   16724 179   0     0        0      7088     13341        0  0x4070244f registrar
 18386   19439 179   1     0        0      6624     12482        0  0x40702f90 registrar
 26001   27050 179   1     0        0      5138     11260        0  0x4070408a registrar
 27379   28427 179   0     0        0      5707     10864        0  0x40704635 registrar
 26391   27443 179   0     0        0      5656     10748        0  0x40704365 registrar
 29240     383 179   0     0        0      5219      9826        0  0x40704bd6 registrar
  1496    2555 179   1     0        0      4780      9064        0  0x4070572a registrar
  2256    3301 179   1     0        0      4456      8478        0  0x40705ccb registrar
  3565    4590 179   1     0        0      3284      8419        0  0x4070680b registrar
  2547    3599 179   1     0        0      4430      8365        0  0x40705f9b registrar
 21986   26310 168   0     0        0     53351      8284       96  0x404bca58 pqexpire
 22157   26481 154   0     0        0     29569      8226       47  0x404bca61 RadarServer
  2907    3963 179   0     0        0      4271      8089        0  0x4070626b registrar
  3750    4772 179   1     0        0      3868      7354        0  0x40706adb registrar
  4434    5456 179   1     0        0      3508      6631        0  0x4070734c registrar
  5333    5522 154   1     0        0     19313      6544      249  0x3fc1bfae armmon
  4598    5632 179   1     0        0      3316      6332        0  0x4070761c registrar
  4735    5768 178   0     0        0      3209      6076        0  0x407078ec registrar
  6105    7121 179   0     0        0      2302      4368        0  0x40708c9d registrar
  6444    7447 179   0     0        0      2269      4271        0  0x40708f6d registrar
  6717    7725 179   1     0        0      2060      3895        0  0x4070923d registrar
  2826   11543 168   0     0        0      2341      3699      297  0x40219a14 NmonDtcResiden
  7392    8404 179   0     0        0      1772      3389        0  0x407097de registrar
  9004   10010 179   0     0        0      1475      2777        0  0x4070a068 registrar
  9251   10259 179   1     0        0      1282      2428        0  0x4070a338 registrar
  9378   10385 179   1     0        0      1185      2234        0  0x4070a609 registrar
  9603   10611 179   1     0        0      1074      2046        0  0x4070a8d9 registrar
 22147   26471 154   1     0        0      5211      1902      106  0x404bca61 RadarMsgHandle
 10113   11119 179   1     0        0       818      1562        0  0x4070ae79 registrar
 10467   11473 179   1     0        0       731      1436        0  0x4070b14a registrar
 10652   11654 179   1     0        0       546      1039        0  0x4070b41a registrar
  5337    5526 154   0     0        0       707       903       11  0x3fc1bfae dm_TL_adapter
 11027   12032 179   0     0        0       462       872        0  0x4070b6ea registrar
 11423   12427 179   0     0        0       329       671        0  0x4070b9bb registrar
  7907     536 154   0     0        0       145       646        2  0x40355adf startWfoApi
  8782   10126 182   0     0        0     24756       603       16  0x406dd24f gzip
  5353    5542 154   0     0        0       386       583        5  0x3fc1bfae lpmc_em
 21940   23259 183   1     0        0     22169       529       15  0x406de915 gzip
  5335    5524 154   0     0        0       398       509        4  0x3fc1bfae dm_FCMS_adapte
  5344    5533 154   0     0        0       371       487        4  0x3fc1bfae dm_memory
 11812   12818 179   1     0        0       224       426        0  0x4070bc8b registrar
 19524   20716 183   0     0        0      6018       172        3  0x406e5b10 gzip
  5581    6729 183   1     0        0        51       137        4  0x406ea23b NdmFp
 11866   12872 179   1     0        0       422       135        1  0x4070bce0 testNotify
 26917   28100 154   0     0        0        46       122        4  0x406e7608 winsFileReso.t
  6746    7756 178   1     0        0        25        91        0  0x407092d5 cwbQPESUMS.ksh
 13426   14566 183   1     0        0        32        87        2  0x406eae48 NdmFp
  2001    2059 154   0     0        0       231        59        0  0x3fc1bd31 registrar
 21326   22465 182   1     0        0        14        52        0  0x406ebc59 NdmFp
  1309    2476 182   1     0        0         2        40        2  0x406ed194 ftp
 11422   12588 154   0     0        0        18        35        1  0x406e8418 winsFileReso.t
 17620   18756 183   1     0        0        69        26        0  0x406eb2b3 GridFileNotify
 29157     402 183   1     0        0        50        22        0  0x406ecf2e GridFileNotify
 29225     471 182   0     0        0         1        20        1  0x406ecf46 ftp
  1337    2504 183   1     0        0        46        18        0  0x406ed197 GridFileNotify
 10902   11907 154   0     0        0         6        15        0  0x4070b5d8 ksh
 10068   11074 158   0     0        0         6        13        0  0x4070aded ksh
 10045   11052 154   0     0        0         2        10        0  0x4070adc4 rlogind
 29015     213 158   0     0        0         3         8        0  0x406fb36b cwbgrid.ksh
  7448    8461 158   1     0        0         3         8        0  0x4070984a cwbgrid.ksh
  5556    6703 182   0     0        0         2         8        0  0x406ea201 Mesonet1hrPrep
 10201   11207 158   0     0        0         1         8        0  0x4070af67 startSAOdecode
 12371   13373 154   0     0        0         4         7        0  0x4070bf6d ksh
 12597   13600 158   1     0        0         2         7        0  0x4070bffa goessat.ksh
 12258   13263 178   0     0        0         2         7        0  0x4070bf1e cwbgrid.ksh
 12220   13226 178   0     0        0         2         7        0  0x4070bf03 cwbgrid.ksh
 12147   13153 178   1     0        0         2         7        0  0x4070beb0 cwbgrid.ksh
 12091   13097 178   0     0        0         2         7        0  0x4070be65 cwbgrid.ksh
  6711    7861 154   0     0        0         2         7        0  0x406ea597 Mesonet1hrPrep
 12162   13168 158   0     0        0         1         7        0  0x4070bec3 awk
 12961   13962 179   0     0        0        78         6        0  0x4070c10c goesasasat
  5540    6687 182   1     0        0         5         6        0  0x406ea1f4 winsFileReso.t
 25088   26245 158   0     0        0         2         6        0  0x406e9403 fxaStopProc.ks
 12347   13350 154   0     0        0         2         6        0  0x4070bf60 cwbgrid.ksh
  3531    4554 158   1     0        0         2         6        0  0x40706783 cwbgrid.ksh
  7350    8460 158   0     0        0         1         6        0  0x406f4c9e remakenfsmc.ks
 15504   16643 154   0     0        0         5         5        0  0x406eaff2 winsFileReso.t
 13053   14194 154   0     0        0         2         5        0  0x406eadc6 Mesonet1hrPrep
 12983   13985 178   0     0        0         1         5        0  0x4070c11c cwbRNC.ksh
 12925   13925 178   1     0        0         1         5        0  0x4070c0f2 cwbRNC.ksh
  7856    8867 154   0     0        0         1         5        0  0x40709a8e gfsgat120
 21322   22461 154   0     0        0         5         4        0  0x406ebc59 winsFileReso.t
 11071   12218 158   0     0        0         2         4        0  0x406eaa63 Mesonet1hrPrep
 23774   24929 158   0     0        0         1         4        0  0x406e9290 chkcomp.ksh
 19924   21058 158   0     0        0         1         4        0  0x406eba5e chkcomppq.ksh
  2650    3810 182   0     0        0         1         4        0  0x406ed271 ftp
  2521    3900 158   1     0        0         1         4        0  0x406dcfa1 bakfxa.ksh
  2114    3276 183   1     0        0         6         3        0  0x406ed223 grep
 29285     531 182   1     0        0         4         3        0  0x406ecf61 winsFileReso.t
  1365    2532 168   0     0        0         3         3        0  0x406ed1a4 ymdd.csh
 28149   29273 183   0     0        0         1         3        0  0x406f1834 ftp
 22775   23931 158   0     0        0         1         3        0  0x406e9145 chkcomp.ksh
 21893   23033 182   1     0        0         1         3        0  0x406ebd84 fxaKillProc.ks
 21562   22701 182   0     0        0         1         3        0  0x406ebce0 Mesonet1hrPrep
 20263   21421 158   0     0        0         1         3        0  0x406e8c86 chkpqact.ksh
 19961   21094 154   0     0        0         1         3        0  0x406eba65 fxaKillProc.ks
 19534   20670 154   0     0        0         1         3        0  0x406eba00 Mesonet1hrPrep
 18172   19310 182   1     0        0         1         3        0  0x406eb618 fxaKillProc.ks
 17624   18760 154   0     0        0         1         3        0  0x406eb2b3 chkldmd.ksh
 13099   14099 178   1     0        0         1         3        0  0x4070c181 cwbRNC.ksh
 13072   14072 158   1     0        0         1         3        0  0x4070c164 cwbSynop.ksh
 12950   13951 178   1     0        0         1         3        0  0x4070c107 UddDecoder.ksh
 11216   12364 182   0     0        0         1         3        0  0x406eaacd chkldmd.ksh
  5486    5675 154   1     0        0         1         3        0  0x3fc1bfb0 dtlogin
  4901    6063 158   0     0        0         1         3        0  0x406e9e6a awk
  2832   11549 154   0     0        0         1         3        0  0x40219a14 NmqSrvd
 13152   14150 178   1     0        0         0         3        0  0x4070c1ab cwbRNC.ksh
 12874   13874 158   0     0        0         0         3        0  0x4070c0c7 startUddDecode
  8668    9811 182   0     0        0         0         3        0  0x406ea7f1 chkldmd.ksh
  1472    2640 183   1     0        0         7         2        0  0x406ed1c0 winsFileFilter
 22017   23153 154   0     0        0         1         2        0  0x406ebde6 chkcomppq.ksh
 16193   17386 158   0     0        0         1         2        0  0x406e58f7 fxaNotifyRelay
 15651   16788 154   0     0        0         1         2        0  0x406eb039 chkcomp.ksh
 15537   16855 158   1     0        0         1         2        0  0x406de54d fxaNotifyRelay
 13199   14198 178   0     0        0         1         2        0  0x4070c1c7 nwpprimaryhost
 12811   13814 178   0     0        0         1         2        0  0x4070c099 cwbgrid2.ksh
  4707    5923 158   1     0        0         1         2        0  0x406e5201 fxaNotifyRelay
  3877    5172 158   1     0        0         1         2        0  0x406dff88 fxaNotifyRelay
  1980    3365 158   1     0        0         1         2        0  0x406dcf6c fxaNotifyRelay
  1392    2560 182   0     0        0         1         2        0  0x406ed1ac fxaStartProc.k
  1343    2510 182   1     0        0         1         2        0  0x406ed199 fxaKillProc.ks
 28392   29552 182   1     0        0         0         2        0  0x406e96f9 ftplastlaps_3k
 25295   26437 158   0     0        0         0         2        0  0x406ec234 sh
 21626   22765 182   1     0        0         0         2        0  0x406ebcf9 wfoApiChk.ksh
 21561   22700 182   1     0        0         0         2        0  0x406ebce0 ckpacct
 17676   18813 182   1     0        0         0         2        0  0x406eb2fd ftplastlaps_3k
 13214   14213 178   1     0        0         0         2        0  0x4070c1d2 cwbRNC.ksh
 10502   11651 182   1     0        0         0         2        0  0x406ea987 chkldmd.ksh
  5274    6434 182   0     0        0         0         2        0  0x406ea039 ftplastlaps_3k
  2115    3277 154   0     0        0         2         1        0  0x406ed223 grep
  6256    7384 154   0     0        0         1         1        0  0x406ed5dd ps
  2552    3713 182   0     0        0         1         1        0  0x406ed261 ps
 29311     561 182   1     0        0         0         1        0  0x406ecf72 wfoApiChk.ksh
 29309     558 158   0     0        0         0         1        0  0x406ecf72 sh
 29110     354 158   0     0        0         0         1        0  0x406ecf19 ftplastmm5_2.k
 28351   29511 158   0     0        0         0         1        0  0x406e96d8 sh
 28193   29317 158   0     0        0         0         1        0  0x406f1897 sh
 28166   29290 182   1     0        0         0         1        0  0x406f184f sh
 28158   29282 158   1     0        0         0         1        0  0x406f1842 sh
 28132   29256 182   0     0        0         0         1        0  0x406f1833 whoami
 25637   26781 158   0     0        0         0         1        0  0x406ec2e8 sh
 24878   26017 158   0     0        0         0         1        0  0x406ec112 sh
 24871   26010 158   0     0        0         0         1        0  0x406ec10e sh
 24861   26001 182   1     0        0         0         1        0  0x406ec10c sh
 24857   25997 158   0     0        0         0         1        0  0x406ec108 sh
 24629   25728 158   1     0        0         0         1        0  0x406f9a2d cpdelcomp.ksh
 24052   25193 158   0     0        0         0         1        0  0x406ec057 ftpmm5_2.ksh
 21591   22729 158   0     0        0         0         1        0  0x406ebced ftplastlaps_3k
 21568   22707 158   0     0        0         0         1        0  0x406ebce0 ftplastlaps_3k
 21328   22467 158   0     0        0         0         1        0  0x406ebc59 ftplastmm5_2.k
 21317   22456 158   0     0        0         0         1        0  0x406ebc59 sh
 21315   22454 158   1     0        0         0         1        0  0x406ebc59 sh
 21313   22452 158   0     0        0         0         1        0  0x406ebc59 sh
 19640   20776 158   0     0        0         0         1        0  0x406eba12 ftplastmm5_2.k
 19598   23922 168   1     0        0         0         1        0  0x404bc9be runpg
 18400   19538 158   0     0        0         0         1        0  0x406eb7b7 ftplastmm5_2.k
 18390   19528 158   0     0        0         0         1        0  0x406eb7b0 sh
 17926   19061 158   0     0        0         0         1        0  0x406eb424 sh
 17675   18811 182   1     0        0         0         1        0  0x406eb2fd wfoApiChk.ksh
 17668   18804 158   0     0        0         0         1        0  0x406eb2f9 sh
 16329   19308 156   0     0        0         0         1        0  0x406bcf1b getty
 13428   14568 182   1     0        0         0         1        0  0x406eae48 ftplastlaps_3k
 13425   14565 182   0     0        0         0         1        0  0x406eae48 ftplastmm5_2.k
 13416   14556 158   0     0        0         0         1        0  0x406eae48 sh
 13414   14554 158   0     0        0         0         1        0  0x406eae48 sh
 13412   14552 158   0     0        0         0         1        0  0x406eae48 sh
 13175   14173 154   0     0        0         0         1        0  0x4070c1ba remsh
 13169   14167 178   0     0        0         0         1        0  0x4070c1b6 cwbgrid2.ksh
 13140   14139 154   0     0        0         0         1        0  0x4070c19e nwpprimary.ksh
 13136   14135 178   0     0        0         0         1        0  0x4070c19c cwbgrid2.ksh
 13011   14013 152   0     0        0         0         1        0  0x4070c132 sh
 13010   14012 182   1     0        0         0         1        0  0x4070c132 sh
 12990   13992 182   1     0        0         0         1        0  0x4070c124 sh
 12695   13698 158   0     0        0         0         1        0  0x4070c04b sh
 12681   13683 182   1     0        0         0         1        0  0x4070c03d sh
 12680   13682 182   0     0        0         0         1        0  0x4070c03d sh
 12606   13609 182   1     0        0         0         1        0  0x4070c001 sh
 12538   13540 182   1     0        0         0         1        0  0x4070bfd8 sh
 12537   13539 183   0     0        0         0         1        0  0x4070bfd8 sh
 12488   13492 182   1     0        0         0         1        0  0x4070bfbe sh
 12487   13491 182   0     0        0         0         1        0  0x4070bfbe sh
 12482   21173 154   0     0        0         0         1        0  0x4046d62b ttd
 12471   13476 183   1     0        0         0         1        0  0x4070bfb1 sh
 12470   13475 182   1     0        0         0         1        0  0x4070bfb1 sh
 12401   13405 182   0     0        0         0         1        0  0x4070bf82 sh
 12365   13368 182   1     0        0         0         1        0  0x4070bf6b sh
 12364   13367 182   0     0        0         0         1        0  0x4070bf6b sh
 12337   13340 182   0     0        0         0         1        0  0x4070bf55 sh
 12336   13339 182   0     0        0         0         1        0  0x4070bf55 sh
 12223   13229 182   1     0        0         0         1        0  0x4070bf03 sh
 12222   13228 182   0     0        0         0         1        0  0x4070bf03 sh
 12120   13126 158   0     0        0         0         1        0  0x4070be8b killpn.ksh
 11713   12719 182   1     0        0         0         1        0  0x4070bbd8 sh
 11615   12621 182   0     0        0         0         1        0  0x4070bb43 sh
 11582   12589 158   0     0        0         0         1        0  0x4070bafc remakenfsrc.ks
 11579   12585 158   1     0        0         0         1        0  0x4070bafc remakenfsmc.ks
 11476   12480 182   1     0        0         0         1        0  0x4070ba29 sh
 11385   12390 182   1     0        0         0         1        0  0x4070b95f sh
 11346   12346 182   0     0        0         0         1        0  0x4070b933 sh
 11332   12332 183   0     0        0         0         1        0  0x4070b91d sh
 11313   12313 182   1     0        0         0         1        0  0x4070b907 sh
 11218   12219 182   0     0        0         0         1        0  0x4070b872 sh
 11208   12356 158   0     0        0         0         1        0  0x406eaac4 sh
 11204   12352 158   0     0        0         0         1        0  0x406eaac4 sh
 11180   12185 182   1     0        0         0         1        0  0x4070b836 sh
 11128   12132 183   0     0        0         0         1        0  0x4070b7c4 sh
 10593   11741 158   0     0        0         0         1        0  0x406ea998 sh
  6679    7829 182   0     0        0         0         1        0  0x406ea567 ftplastlaps_3k
  6615    7764 158   0     0        0         0         1        0  0x406ea4e8 sh
  6466    7595 182   0     0        0         0         1        0  0x406ed6ad sh
  6429    7558 182   0     0        0         0         1        0  0x406ed690 fxaKillProc.ks
  6342    7470 152   0     0        0         0         1        0  0x406ed620 sh
  6283    7411 158   0     0        0         0         1        0  0x406ed5ed ftplaps_3km.ks
  6194    7323 158   0     0        0         0         1        0  0x406ed59b ftplaps_3kmls.
  3722    4744 158   1     0        0         0         1        0  0x40706a59 cwbgrid
  2777    3957 158   0     0        0         0         1        0  0x406e9b89 sh
  2658    3818 158   0     0        0         0         1        0  0x406ed273 chkprocess.ksh
  2554    3715 158   0     0        0         0         1        0  0x406ed261 ftpmm5_2.ksh
  2287    3449 182   1     0        0         0         1        0  0x406ed242 killpn.ksh
  2283    3445 158   1     0        0         0         1        0  0x406ed242 sh
  2193    3355 158   0     0        0         0         1        0  0x406ed233 chkprocess.ksh
  2189    3351 154   0     0        0         0         1        0  0x406ed233 chkldmd.ksh
  2164    3326 158   0     0        0         0         1        0  0x406ed22e sh
  2070    3231 182   1     0        0         0         1        0  0x406ed21c wfoApiChk.ksh
  2062    3200 158   0     0        0         0         1        0  0x406ed219 sh
  1556    2715 158   0     0        0         0         1        0  0x406ed1c9 sh
  1474    2642 158   0     0        0         0         1        0  0x406ed1c0 sh
  1370    2537 154   0     0        0         0         1        0  0x406ed1a4 chkldmd.ksh
  1364    2531 154   0     0        0         0         1        0  0x406ed1a4 cut
  1345    2512 158   0     0        0         0         1        0  0x406ed19a ftplastlaps_3k
  1305    2472 182   1     0        0         0         1        0  0x406ed194 ckpacct
  1293    2460 154   0     0        0         0         1        0  0x406ed194 sh
  1132    2272 158   0     0        0         0         1        0  0x406fc03a cwbgrid
   905    2093 178   1     0        0         0         1        0  0x406ed160 sh
 29387     680 179   1     0        0         0         0        0  0x406e781c registrar
 29302     551 158   0     0        0         0         0        0  0x406ecf69 sh
 29262     508 182   0     0        0         0         0        0  0x406ecf59 sh
 29186     431 158   0     0        0         0         0        0  0x406ecf30 ftpmm5_2.ksh
 29113     357 154   0     0        0         0         0        0  0x406ecf19 chkprocess.ksh
 28989     451 182   0     0        0         0         0        0  0x406dce03 gzip
 28502   29626 182   1     0        0         0         0        0  0x406f1b93 chmod
 28500   29624 182   0     0        0         0         0        0  0x406f1b93 ftp
 28499   29623 154   0     0        0         0         0        0  0x406f1b93 grep
 28498   29622 182   0     0        0         0         0        0  0x406f1b93 ps
 28492   29616 182   1     0        0         0         0        0  0x406f1b8c winsFileFilter
 28491   29615 182   0     0        0         0         0        0  0x406f1b8b hostname
 28489   29613 182   1     0        0         0         0        0  0x406f1b8b wfoApiChk.ksh
 28488   29612 182   1     0        0         0         0        0  0x406f1b8b sh
 28194   29318 182   1     0        0         0         0        0  0x406f1897 ckpacct
 28162   29286 182   1     0        0         0         0        0  0x406f1843 date
 28161   29285 154   0     0        0         0         0        0  0x406f1843 sa1
 28152   29276 168   1     0        0         0         0        0  0x406f1834 memo
 28150   29274 182   1     0        0         0         0        0  0x406f1834 memo
 28148   29272 182   0     0        0         0         0        0  0x406f1834 ftp
 28146   29270 182   1     0        0         0         0        0  0x406f1833 find
 28144   29268 182   0     0        0         0         0        0  0x406f1833 ftplaps_3km.ks
 28142   29266 182   1     0        0         0         0        0  0x406f1833 sh
 28136   29260 182   0     0        0         0         0        0  0x406f1833 grep
 28134   29258 182   0     0        0         0         0        0  0x406f1833 rm
 28130   29254 154   0     0        0         0         0        0  0x406f1833 grep
 28127   29251 154   0     0        0         0         0        0  0x406f1833 grep
 28126   29250 182   0     0        0         0         0        0  0x406f1833 chkprocess.ksh
 28125   29249 154   0     0        0         0         0        0  0x406f1833 grep
 28124   29248 154   0     0        0         0         0        0  0x406f1833 grep
 28122   29246 182   1     0        0         0         0        0  0x406f1833 sh
 28119   29865 179   1     0        0         0         0        0  0x406d04b9 registrar
 27691   27880 154   0     0        0         0         0        0  0x3fc1c188 hpacbcol
 27592   28715 179   0     0        0         0         0        0  0x406f132d registrar
 27191   28445 179   0     0        0         0         0        0  0x406e2ef5 registrar
 25880   27241 158   1     0        0         0         0        0  0x406dcc9d fxaNotifyRelay
 25296   26438 182   0     0        0         0         0        0  0x406ec234 sh
 25273   26397 179   1     0        0         0         0        0  0x406f024b registrar
 24971   26111 179   1     0        0         0         0        0  0x406ec14a registrar
 24108   25249 183   1     0        0         0         0        0  0x406ec063 GridFileNotify
 23887   25046 183   0     0        0         0         0        0  0x406e92a9 NdmFp
 23305   24459 154   0     0        0         0         0        0  0x406e9228 winsFileReso.t
 22936   24091 158   0     0        0         0         0        0  0x406e91bd chkcomp.ksh
 22836   23991 179   0     0        0         0         0        0  0x406e916f registrar
 21746   22885 158   0     0        0         0         0        0  0x406ebd24 find
 20848   22004 186   1     0        0         0         0        0  0x406e8e11 startWfoApi
 20807   21945 179   1     0        0         0         0        0  0x406ebba3 registrar
 20701   25024 168   0     0        0         0         0        0  0x404bca2f pqact
 20639   24963 168   0     0        0         0         0        0  0x404bca29 NotifyLog
 20402   21633 179   0     0        0         0         0        0  0x406e3a36 registrar
 19949   21120 179   0     0        0         0         0        0  0x406e6fab registrar
 19826   24150 168   0     0        0         0         0        0  0x404bc9c4 pqexpire
 18552   19689 179   0     0        0         0         0        0  0x406eb8d2 registrar
 18398   19536 158   1     0        0         0         0        0  0x406eb7b7 sh
 18024   19160 158   0     0        0         0         0        0  0x406eb4d8 sh
 17780   19242 179   1     0        0         0         0        0  0x406d7ae4 registrar
 17700   18837 154   0     0        0         0         0        0  0x406eb30a complapsp.ksh
 17674   18810 182   0     0        0         0         0        0  0x406eb2fd fxaKillProc.ks
 17666   18802 158   0     0        0         0         0        0  0x406eb2f8 sh
 17567   18812 179   1     0        0         0         0        0  0x406e2955 registrar
 17543   19146 179   0     0        0         0         0        0  0x406d375f registrar
 15712   17277 179   1     0        0         0         0        0  0x406d4840 registrar
 13575   14677 179   1     0        0         0         0        0  0x406f787f registrar
 13213   14212 178   0     0        0         0         0        0  0x4070c1d1 cat
 13212   14211 178   1     0        0         0         0        0  0x4070c1d1 wc
 13210   14209 149   1     0        0         0         0        0  0x4070c1d0 cwbgrid2.ksh
 13209   14208 178   0     0        0         0         0        0  0x4070c1d0 awk
 13208   14207 134   1     0        0         0         0        0  0x4070c1d0 cwbQPESUMS.ksh
 13207   14206 178   0     0        0         0         0        0  0x4070c1d0 cut
 13204   14203 178   1     0        0         0         0        0  0x4070c1cc mv
 13202   14201 178   1     0        0         0         0        0  0x4070c1c9 sh
 13197   14196 154   0     0        0         0         0        0  0x4070c1c7 remshd
 13196   14195 179   1     0        0         0         0        0  0x4070c1c6 saoDecoder
 13192   14190 178   1     0        0         0         0        0  0x4070c1c3 cwbgrid2.ksh
 13188   14186 178   1     0        0         0         0        0  0x4070c1c2 date
 13182   14180 149   1     0        0         0         0        0  0x4070c1bc cwbgrid.ksh
 13181   14179 178   0     0        0         0         0        0  0x4070c1bc cwbgrid.ksh
 13157   14155 178   1     0        0         0         0        0  0x4070c1ad cwbgrid2.ksh
 13122   14122 158   1     0        0         0         0        0  0x4070c192 sh
 13081   14081 154   0     0        0         0         0        0  0x4070c167 cwbgrid2.ksh
 13012   14014 182   1     0        0         0         0        0  0x4070c132 sh
 12991   13993 182   0     0        0         0         0        0  0x4070c124 sh
 12938   13939 179   1     0        0         0         0        0  0x4070c0f6 nfsrc
 12937   13938 158   1     0        0         0         0        0  0x4070c0f6 sh
 12872   13872 168   0     0        0         0         0        0  0x4070c0c5 sleep
 12856   13999 179   1     0        0         0         0        0  0x406ead91 registrar
 12730   13732 182   1     0        0         0         0        0  0x4070c067 sh
 12707   21399 154   0     0        0         0         0        0  0x4046d633 perflbd
 12697   13700 182   0     0        0         0         0        0  0x4070c04b sa1
 12694   13697 182   0     0        0         0         0        0  0x4070c04b sh
 12607   13610 182   0     0        0         0         0        0  0x4070c001 sh
 12550   13552 182   1     0        0         0         0        0  0x4070bfe5 sh
 12449   13453 182   1     0        0         0         0        0  0x4070bfa3 sh
 12366   13369 182   0     0        0         0         0        0  0x4070bf6b sh
 12339   13342 154   1     0        0         0         0        0  0x4070bf57 telnetd
 12303   13306 182   0     0        0         0         0        0  0x4070bf3f sh
 12228   13574 183   0     0        0         0         0        0  0x406dd3b9 tar
 12176   13182 178   1     0        0         0         0        0  0x4070beda cwbQPESUMS.ksh
 12167   13173 154   0     0        0         0         0        0  0x4070becc ksh
 12159   13165 154   1     0        0         0         0        0  0x4070bebd telnetd
 12153   13159 179   1     0        0         0         0        0  0x4070beb7 nfsrc
 12009   20700 154   1     0        0         0         0        0  0x4046d60b dced
 11865   12871 158   1     0        0         0         0        0  0x4070bce0 sh
 11777   12846 179   1     0        0         0         0        0  0x40700dce registrar
 11661   12667 158   1     0        0         0         0        0  0x4070bb8e nfskb4
 11660   12666 158   0     0        0         0         0        0  0x4070bb8e sh
 11658   12664 182   0     0        0         0         0        0  0x4070bb8d sh
 11591   12598 158   1     0        0         0         0        0  0x4070bb0b remakenfshc.ks
 11398   12403 182   0     0        0         0         0        0  0x4070b97d hostname
 11352   12355 182   1     0        0         0         0        0  0x4070b939 hostname
 11347   12347 182   0     0        0         0         0        0  0x4070b933 cron
 11334   12334 182   0     0        0         0         0        0  0x4070b91d sh
 11333   12333 152   1     0        0         0         0        0  0x4070b91d sh
 11276   12278 182   0     0        0         0         0        0  0x4070b8db sh
 11275   12277 152   1     0        0         0         0        0  0x4070b8db sh
 11259   12261 182   0     0        0         0         0        0  0x4070b8c4 sh
 11109   12256 158   0     0        0         0         0        0  0x406eaa79 awk
 10958   11963 182   0     0        0         0         0        0  0x4070b65c sh
 10881   11886 154   0     0        0         0         0        0  0x4070b5b4 rlogind
 10605   11753 182   0     0        0         0         0        0  0x406ea999 ftplastmm5_2.k
 10599   11747 182   1     0        0         0         0        0  0x406ea999 ftplastlaps_3k
 10595   11743 158   0     0        0         0         0        0  0x406ea998 sh
 10578   12271 179   0     0        0         0         0        0  0x406d159b registrar
  9905   10914 178   1     0        0         0         0        0  0x4070ac10 cwbQPESUMS.ksh
  9512   10621 179   1     0        0         0         0        0  0x406f5f2c registrar
  7855    8866 158   1     0        0         0         0        0  0x40709a8e sh
  7854    8865 158   0     0        0         0         0        0  0x40709a8e cwbgrid
  7036    8238 183   1     0        0         0         0        0  0x406e5319 gzip
  7007    8022 178   1     0        0         0         0        0  0x4070950e registrar
  6621    7770 182   0     0        0         0         0        0  0x406ea4e8 ftplastlaps_3k
  6469    7598 158   0     0        0         0         0        0  0x406ed6b0 ftplaps_3km.ks
  6468    7597 182   1     0        0         0         0        0  0x406ed6b0 Mesonet1hrPrep
  6467    7596 182   0     0        0         0         0        0  0x406ed6b0 cut
  6461    7590 154   0     0        0         0         0        0  0x406ed6ac grep
  6460    7589 154   0     0        0         0         0        0  0x406ed6ac grep
  6459    7588 154   0     0        0         0         0        0  0x406ed6ac grep
  6456    7585 182   0     0        0         0         0        0  0x406ed6ac hostname
  6438    7567 182   1     0        0         0         0        0  0x406ed697 winsFileReso.t
  6428    7557 182   0     0        0         0         0        0  0x406ed690 awk
  6418    7547 182   0     0        0         0         0        0  0x406ed674 chkprocess.ksh
  6417    7546 154   0     0        0         0         0        0  0x406ed674 chkldmd.ksh
  6395    7523 154   0     0        0         0         0        0  0x406ed659 grep
  6394    7522 182   1     0        0         0         0        0  0x406ed659 grep
  6267    7395 182   0     0        0         0         0        0  0x406ed5e4 hostname
  6265    7393 182   0     0        0         0         0        0  0x406ed5e4 sh
  6197    7326 182   0     0        0         0         0        0  0x406ed59c sh
  6187    7316 158   0     0        0         0         0        0  0x406ed59b ftplastlaps_3k
  6142    7271 179   1     0        0         0         0        0  0x406ed50e registrar
  5952    7244 182   1     0        0         0         0        0  0x406e00d5 gzip
  5631    6646 178   1     0        0         0         0        0  0x407086fd registrar
  5357    5546 154   0     0        0         0         0        0  0x3fc1bfaf sysstat_em
  5347    5536 154   0     0        0         0         0        0  0x3fc1bfae dm_stape
  5339    5528 154   0     0        0         0         0        0  0x3fc1bfae dm_core_hw
  5321    5510 154   0     0        0         0         0        0  0x3fc1bfae disk_em
  5310    5499 168   1     0        0         0         0        0  0x3fc1bfad p_client
  5256    6416 158   1     0        0         0         0        0  0x406ea038 sh
  5106    6196 179   0     0        0         0         0        0  0x406fe0a0 registrar
  4900    5934 179   1     0        0         0         0        0  0x40707bbd registrar
  4441    5909 179   1     0        0         0         0        0  0x406d8085 registrar
  4164    5296 182   1     0        0         0         0        0  0x406ed350 sh
  3701    4782 179   0     0        0         0         0        0  0x406fd55e registrar
  3323    3504 154   1     0        0         0         0        0  0x3fc1bf7f camf
  3192    3255 158   0     0        0         0         0        0  0x3fc1bf72 dtrc
  3168    3226 154   0     0        0         0         0        0  0x3fc1bf70 nfsd
  3163    3221 154   0     0        0         0         0        0  0x3fc1bf70 nfsd
  3161    3219 154   0     0        0         0         0        0  0x3fc1bf70 nfsd
  3158    3216 154   0     0        0         0         0        0  0x3fc1bf70 nfsd
  3153    3211 154   0     0        0         0         0        0  0x3fc1bf70 nfsd
  3151    3209 154   1     0        0         0         0        0  0x3fc1bf70 nfsd
  3150    3208 154   0     0        0         0         0        0  0x3fc1bf70 nfsd
  3148    3206 154   1     0        0         0         0        0  0x3fc1bf70 nfsd
  3128    3186 154   1     0        0         0         0        0  0x3fc1bf70 rpc.mountd
  2672    3832 182   0     0        0         0         0        0  0x406ed274 grep
  2659    3819 154   0     0        0         0         0        0  0x406ed273 awk
  2654    3814 182   0     0        0         0         0        0  0x406ed272 date
  2652    3812 183   1     0        0         0         0        0  0x406ed272 winsFileFilter
  2553    3714 182   1     0        0         0         0        0  0x406ed261 sh
  2456    3619 154   0     0        0         0         0        0  0x406ed254 grep
  2455    3618 154   0     0        0         0         0        0  0x406ed254 grep
  2454    3617 154   0     0        0         0         0        0  0x406ed254 grep
  2453    3616 182   0     0        0         0         0        0  0x406ed254 grep
  2356    3519 158   0     0        0         0         0        0  0x406ed24a killpn.ksh
  2354    3517 158   0     0        0         0         0        0  0x406ed24a sh
  2267    3429 182   0     0        0         0         0        0  0x406ed23f tail
  2250    3413 182   0     0        0         0         0        0  0x406ed23b wfoApiChk.ksh
  2249    3412 154   0     0        0         0         0        0  0x406ed23b wfoApiChk.ksh
  2204    3367 158   0     0        0         0         0        0  0x406ed235 ftpmm5_2ls.ksh
  2203    3366 158   0     0        0         0         0        0  0x406ed235 ftplastmm5_2.k
  2188    3350 182   0     0        0         0         0        0  0x406ed233 sh
  2186    3348 154   0     0        0         0         0        0  0x406ed233 awk
  2151    3313 158   0     0        0         0         0        0  0x406ed225 ckpacct
  2146    3308 158   0     0        0         0         0        0  0x406ed225 chkprocess.ksh
  2137    3299 182   0     0        0         0         0        0  0x406ed224 ls
  2136    3298 154   0     0        0         0         0        0  0x406ed224 cut
  2028    3164 182   1     0        0         0         0        0  0x406ed214 awk
  2021    3157 183   1     0        0         0         0        0  0x406ed214 ftp
  2016    3152 152   0     0        0         0         0        0  0x406ed212 sh
  1995    2053 154   0     0        0         0         0        0  0x3fc1bd2f memlogd
  1891    1949 154   0     0        0         0         0        0  0x3fc1bd01 hpnpd
  1881    1939 154   1     0        0         0         0        0  0x3fc1bd00 emsagent
  1821    2965 182   0     0        0         0         0        0  0x406ed1f1 ymdd.csh
  1819    2963 154   0     0        0         0         0        0  0x406ed1f1 cut
  1812    2956 154   0     0        0         0         0        0  0x406ed1f1 chkpqact.ksh
  1786    2912 179   1     0        0         0         0        0  0x406f2c7f registrar
  1776    1833 154   1     0        0         0         0        0  0x3fc1bced swagentd
  1766    1823 154   1     0        0         0         0        0  0x3fc1bcec ARMServer
  1746    1803 158   0     0        0         0         0        0  0x3fc1bceb arraymond
  1662    1719 154   0     0        0         0         0        0  0x3fc1bce0 envd
  1656    1713 154   0     0        0         0         0        0  0x3fc1bce0 psmond
  1651    1708 154   0     0        0         0         0        0  0x3fc1bcde diagmond
  1628    1685 154   0     0        0         0         0        0  0x3fc1bcdb cron
  1617    1674 154   1     0        0         0         0        0  0x3fc1bcdb lpsched
  1596    1653 154   0     0        0         0         0        0  0x3fc1bcda pwgrd
  1582    1639 120   1     0        0         0         0        0  0x3fc1bcda xntpd
  1570    1627  64   0     0        0         0         0        0  0x3fc1bcda rbootd
  1554    2713 158   0     0        0         0         0        0  0x406ed1c9 sh
  1532    1589 154   0     0        0         0         0        0  0x3fc1bcd5 trapdestagt
  1524    1581 154   0     0        0         0         0        0  0x3fc1bcd5 mib2agt
  1516    1573 154   0     0        0         0         0        0  0x3fc1bcd5 hp_unixagt
  1508    1565 154   0     0        0         0         0        0  0x3fc1bcd5 atmsubagt
  1497    1554 154   0     0        0         0         0        0  0x3fc1bcca snmpdm
  1489    1546 168   0     0        0         0         0        0  0x3fc1bcca sendmail
  1488    2653 158   0     0        0         0         0        0  0x406ed1c4 chkprocess.ksh
  1366    2533 154   0     0        0         0         0        0  0x406ed1a4 awk
  1330    2497 158   0     0        0         0         0        0  0x406ed196 ftpmm5_2.ksh
  1249    2399 179   1     0        0         0         0        0  0x406f29af registrar
  1223    1280 154   0     0        0         0         0        0  0x3fc1bcc7 inetd
  1187    1244 154   1     0        0         0         0        0  0x3fc1bc3a rpc.lockd
  1181    1238 154   1     0        0         0         0        0  0x3fc1bc3a rpc.statd
  1172    1229 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1171    1228 154   0     0        0         0         0        0  0x3fc1bc39 biod
  1170    1227 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1169    1226 154   0     0        0         0         0        0  0x3fc1bc39 biod
  1168    1225 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1167    1224 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1166    1223 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1165    1222 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1164    1221 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1163    1220 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1162    1219 154   0     0        0         0         0        0  0x3fc1bc39 biod
  1161    1218 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1160    1217 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1159    1216 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1158    1215 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1157    1214 154   1     0        0         0         0        0  0x3fc1bc39 biod
  1138    1195 152   0     0        0         0         0        0  0x3fc1bc39 nfskd
  1133    1190 154   0     0        0         0         0        0  0x3fc1bc39 rpcbind
  1115    1172 154   1     0        0         0         0        0  0x3fc1bc38 sshd
   908    2096 182   0     0        0         0         0        0  0x406ed160 hostname
   904    2092 182   0     0        0         0         0        0  0x406ed160 rm
   709    2241 179   1     0        0         0         0        0  0x406d7544 registrar
   673    1837 182   1     0        0         0         0        0  0x406f25ec date
   672    1836 128   0     0        0         0         0        0  0x406f25ec notigrid.ksh
   671    1835 182   0     0        0         0         0        0  0x406f25ec chkldmd.ksh
   670    1834 182   0     0        0         0         0        0  0x406f25ec awk
   669    1832 182   0     0        0         0         0        0  0x406f25ec cat
   668    1831 154   0     0        0         0         0        0  0x406f25ec wc
   667    1830 182   1     0        0         0         0        0  0x406f25ec mv
   666    1829 182   0     0        0         0         0        0  0x406f25eb Mesonet1hrPrep
   665    1828 182   1     0        0         0         0        0  0x406f25eb Mesonet1hrPrep
   574     631 154   0     0        0         0         0        0  0x3fc1bc25 atminitd
   570     627 154   0     0        0         0         0        0  0x3fc1bc25 atmstrsrvd
   568     625 154   0     0        0         0         0        0  0x3fc1bc25 atmstrsrvd
   567     624 101   1     0        0         0         0        0  0x3fc1bc25 nicdaemon
   512     569 127   1     0        0         0         0        0  0x3fc1bc20 ntl_reader
   502     559 127   0     0        0         0         0        0  0x3fc1bc1e nktl_daemon
   489     546 155   1     0        0         0         0        0  0x3fc1bc1d ptydaemon
   486     543 154   0     0        0         0         0        0  0x3fc1bc1c syslogd
   360    1489 179   0     0        0         0         0        0  0x406fb93c registrar
    32      89 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      88 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      87 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      86 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      85 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      84 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      83 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      82 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      81 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      80 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      79 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      78 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      77 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      76 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      75 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      74 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      73 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      72 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      71 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      70 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      69 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      68 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      67 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      66 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      65 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      64 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      63 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      62 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      61 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      60 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      59 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      58 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      57 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      56 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      55 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      54 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      53 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      52 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      51 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      50 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      49 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      48 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      47 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      46 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      45 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      44 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      43 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      42 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      41 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      40 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      39 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      38 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      37 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      36 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      35 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      34 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    32      33 138   1     0        0         0         0        0  0x3fc1bbec vxfsd
    32      32 138   0     0        0         0         0        0  0x3fc1bbec vxfsd
    27      27 100   0     0        0         0         0        0  0x3fc1bbeb sblksched
    26      26 100   1     0        0         0         0        0  0x3fc1bbeb sblksched
    25      25 100   0     0        0         0         0        0  0x3fc1bbeb smpsched
    24      24 100   0     0        0         0         0        0  0x3fc1bbeb smpsched
    23      23 147   1     0        0         0         0        0  0x3fc1bbeb lvmkd
    22      22 147   1     0        0         0         0        0  0x3fc1bbeb lvmkd
    21      21 147   1     0        0         0         0        0  0x3fc1bbeb lvmkd
    20      20 147   1     0        0         0         0        0  0x3fc1bbeb lvmkd
    19      19 147   1     0        0         0         0        0  0x3fc1bbeb lvmkd
    18      18 147   1     0        0         0         0        0  0x3fc1bbeb lvmkd
    12      12 -32   0     0        0         0         0        0  0x3fc1bbea ttisr
    11      11 100   0     0        0         0         0        0  0x3fc1bbea strfreebd
    10      10 100   0     0        0         0         0        0  0x3fc1bbea strweld
     9       9 100   0     0        0         0         0        0  0x3fc1bbea strmem
     8       8 100   0     0        0         0         0        0  0x3fc1bbea supsched
     4       4 128   1     0        0         0         0        0  0x3fc1bbea unhashdaemon
     2       2 128   1     0        0         0         0        0  0x3fc1bbea vhand
     1       1 168   1     0        0         0         0        0  0x3fc1bbeb init
     0       0 128   1     0        0         0         0        0  0x3fc1bbea swapper

```

