# System hang with timeout call with negative time.

## mpt driver timeout call with negative time.

```

System crash dump analysis report
=================================

Symptom
-------
   System hang TOC dump be taken for root cause finding.

Root Cause
----------
  mpt driver timeout call with negative time, with following message
  be logged on kernel message buffer, which lead system behavor as
  hang.

  Timeout called with negative time.
  function == 0xE0000000004F0AC0, arg == 0xE000000129FE4800,
  ticks == 0xFF1EF679, flags == 0x0

  The timeout callback function name is mpt_os_callback.
  This timeout will not expire when lbolt is rolled over.
  mpt_os_callback() does not check if the lbolt was rolled over.
  Thus, if the timeout occures just before the roll over of lbolt then
  checked after the lbolt roll over, the timeout does not recognize it
  and never expires.


Action Plan
-----------
  Please upgrade the mpt driver to latest version.
  The current version of mpt driver is 11.23.0712,
  which can be download from

  http://h20293.www2.hp.com/portal/swdepot/displayProductInfo.do?productNumber=A7173A

Detail Analysis
---------------

                        =======================
                        = General Information =
                        =======================

Dump time Mon Oct 20 09:25:46 2008 UTC-8
System has been up 498 days, 19 hours, 52 minutes.

Node Name         : localhost
Model             : server rx4640
BIOS revision     : 04.10
HP-UX version     : B.11.23
Kernel whatstring : @(#) $Revision: vmunix:
                    B11.23_LR FLAVOR=perf Fri Aug 29 22:35:38 PDT 2003 $
Number of CPU's   : 4
Disabled CPU's    : 0
CPU type          : IA64 (1.6 Ghz)
CPU Architecture  : IA-64 0
Load average      : 0.00  0.00  0.00

                        ================
                        = Crash Events =
                        ================


Crash event 0 was a INIT !
This seems to be a user initiated INIT !

                ===========================================
                = Message Buffer (in chronological order) =
                ===========================================


Timeout called with negative time.
function == 0xE0000000004F0AC0, arg == 0xE000000129FE4800,
ticks == 0xFF1EF679,  flags == 0x0

Timeout called with negative time.
function == 0xE0000000004F0AC0, arg == 0xE000000129FE4C00,
ticks == 0xFF1EF64E,  flags == 0x0

                        ==================
                        = Memory Globals =
                        ==================

Physical Memory     = 1041038 pages (3.97 GB)
Free Memory         = 92164 pages (360.02 MB)
Average Free Memory = 92135 pages (359.90 MB)
gpgslim             = 3072 pages (12.00 MB)
lotsfree            = 16384 pages (64.00 MB)
desfree             = 3072 pages (12.00 MB)
minfree             = 1280 pages (5.00 MB)


Memory Resource Group Summary
=============================

mrg        phys pages used pages free pages  description
------     ---------- ---------- ----------  -----------
SYS/0         1041038     948874      92164  System memory
KERN/0         988987     329508     659479  Kernel memory
KERN/1          52051      52051          0  Buffer cache
USER/0         928764     567315     361449  User memory

                        ========================
                        = Buffer Cache Globals =
                        ========================

dbc_max_pct           = 5 %
dbc_min_pct           = 2 %
dbc current pct       = 5.0 %
bufpages              = 52051 pages (203.32 MB)
Number of buf headers = 47802

fixed_cache_size = 0
dbc_parolemem    = 0
dbc_stealavg     = 8161
dbc_ceiling      = 52051 pages (203.32 MB)
dbc_nbuf         = 10410
dbc_bufpages     = 20820 pages (81.33 MB)
dbc_vhandcredit  = 23184205
orignbuf         = 0
origbufpages     = 0 pages


buffer free list counts (bfree_cnts)
====================================

index BQ_LRU    BQ_AGE    BQ_EMPTY  BQ_SENDFILE
----- --------  --------  --------  -----------
0     26644480  8171520   0         0
1     26648576  86777856  0         0
2     26656768  7843840   0         0
3     26648576  3760128   0         0

                        ====================
                        = Swap Information =
                        ====================

swapinfo -mt emulation
======================

             Mb      Mb      Mb   PCT  START/      Mb
TYPE      AVAIL    USED    FREE  USED   LIMIT RESERVE  PRI  NAME
dev        8192     515    7677    6%       0       -    1  LVM vg00/lv2
reserve       -    3234   -3234
memory     4066    1296    2770   32%
total     12258    5045    7213   41%       -       0    -



                =======================================
                = Global Error Counters / kmem_writes =
                =======================================

scsi_bioerrors                    = 2322
scsi_bioerrors_logged             = 0
scsi_async_write_bioerrors        = 0
scsi_async_write_bioerrors_logged = 0
sd_strategy_bioerrors             = 2322
sd_strategy_async_write_bioerrors = 0
sd_epowerf_bioerrors              = 0
sd_epowerf_async_write_bioerrors  = 0


                        ====================
                        = IOVA Usage Check =
                        ====================


97% of IOVA still available/free.

                =======================================
                = Crash Event / Processor Information =
                =======================================

Number of processors = 4
                                           c
                 spin                  b r p
evt cpu type     dpth ipsr             n i l
--- --- ----     ---- ----             - - -
0   3   INIT     0    12100862e01a     1 1 0 it rt di pp dt pk i ic mfl ac be
1   2   INIT     0    12100862e01a     1 1 0 it rt di pp dt pk i ic mfl ac be
2   1   INIT     0    12100862e01a     1 1 0 it rt di pp dt pk i ic mfl ac be
3   0   INIT     0    1010086ae01a     1 0 0 it rt di pp dfh dt pk i ic mfl ac b
e

Pending External Interrupts
===========================

                        Handler
cpu bit shared SPL      SPL      Handler(arg)
--- --- ------ ---      ---      ------------
0   31  N      SPLSCHED SPLSCHED mpsched_interrupt(0x0)
0   192 Y      SPLSCHED SPLIO    asio0_intr(0xe000000129f93800)
0   192 Y      SPLSCHED SPLIO    asio0_intr(0xe000000129f93400)
0   237 N      SPLSCHED SPLDBG   cr_itm_int(0x0)
1   31  N      SPLSCHED SPLSCHED mpsched_interrupt(0x0)
1   237 N      SPLSCHED SPLDBG   cr_itm_int(0x0)
2   31  N      SPLSCHED SPLSCHED mpsched_interrupt(0x0)
2   237 N      SPLSCHED SPLDBG   cr_itm_int(0x0)
3   31  N      SPLSCHED SPLSCHED mpsched_interrupt(0x0)
3   191 N      SPLSCHED SPLIO    iether_isr(0xe000000129fe2040)
3   237 N      SPLSCHED SPLDBG   cr_itm_int(0x0)

SPL/TPR values:

0   = SPLUSER  Default user mode SPL level.
16  = SPLSCHED Disable scheduling interrupts.
96  = SPLSOFT  Disable software interrupts (sw triggers off).
192 = SPLIO    Disable IO modules.
208 = SPLSYS   Disable system events (eg. hardclock).
240 = SPL7     Disable all external interrupts.


                        ========================
                        = Processor Clock Info =
                        ========================

itick_per_tick = 15996628
monarch cpu = 0
monarch Interval Time Counter = 0xf4ed88df1e8116
monarch_time_count_marker = 0xf4ed8729d2085e
monarch secs:ticks late = 4:58
ticks_since_boot = 14748086 (0xe109b6)
adjusted ticks_since_boot = 14748544 (0xe10b80)

    event    mpinfo                secs:ticks ipsr
cpu type     prev_ticks_since_boot clock late i    tpr spl
--- -------- --------------------- ---------- ---- --- ------
0   INIT     14748086                   4:58  1    16  SPLSCHED
1   INIT     14748085                   4:59  1    16  SPLSCHED
2   INIT     14748086                   4:58  1    16  SPLSCHED
3   INIT     14748086                   4:58  1    16  SPLSCHED

                ===============================
                = Physical Processor Location =
                ===============================

CPU type: McKinley Madison (cannot determine if Mx2)

                        cpu
cpu  LID                slot path       spu_state
---- ----               ---- ----       ---------
0    0x0000000000000000 0    120        SPU_ENABLED
1    0x0000000001000000 1    121        SPU_ENABLED
2    0x0000000002000000 2    122        SPU_ENABLED
3    0x0000000003000000 3    123        SPU_ENABLED

                        =================
                        = Syswait Array =
                        =================

cpu iowait
--- ------
0   14
1   11
2   3
3   12

This shows the number of threads waiting on buffer I/O.

                        =================
                        = Load Averages =
                        =================


avenrun
=======
0.00  0.00  0.00

mp_avenrun
==========
cpu0 : 0.000001  0.000023  0.000021
cpu1 : 0.000001  0.000135  0.000318
cpu2 : 0.000000  0.000029  0.000027
cpu3 : 0.000000  0.000000  0.000002

                        ======================
                        = Thread Information =
                        ======================

40 Threads ran in the last second
64 Threads ran in the last 5 seconds
69 Threads ran in the last 10 seconds
82 Threads ran in the last minute
170 Threads ran in the last hour

statdaemon ran 89 ticks ago


Most Common Wait Channels
=========================

                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
vx_rwsleep_lock(0xe00000013962a681)                244    14737728   13279485
0xe0000001247326a8                                 138    14733810   82064
vx_rwsleep_rec_lock(0xe0000001384093a8)            30     35132      14776
vx_worklist_thread_sv                              18     159        59
async_bufhead                                      16     14738567   14738567
*per_processor_selects+0x80                        8      3357940821 275
*per_processor_selects+0xc0                        8      1791752750 85
*per_processor_selects+0x0                         8      1440732218 47
*per_processor_selects+0x40                        7      15093972   152
lvmkd_q                                            6      14748558   14748558
schedcpu_procp                                     6      238        38
vx_logflush(0xe00000012bf0c280)                    5      14745159   14605659
svc_run(0xe0000001283360c0)                        4      14734789   14734789
svc_run(0xe000000128336080)                        4      14734789   14734789
svc_run(0xe000000128336040)                        4      14734789   14734789
svc_run(0xe000000128336100)                        4      14734789   14734789
streams_mp_sync                                    4      317        17
vx_bc_buf_locksleep(0xe000000130fb33b8)            3      14743859   14743859
ken_recv_msg(0xe0000001282f2040)                   3      14743094   14743092
vx_bc_buf_locksleep(0xe000000130e86fd8)            3      14739459   14739359
vx_bc_buf_locksleep(0xe000000130e9c6d8)            3      14739259   14739159
0xe000000138411128                                 3      14735781   14680339
LVM vg00/lv8                                       2      14748086   14725919
vx_bc_buf_locksleep(0xe000000130fe7db8)            2      14746559   14740559
vx_bc_buf_locksleep(0xe000000130e08918)            2      14746459   14740986
vx_bc_buf_locksleep(0xe000000130ec74d8)            2      14745059   14708959
streams_blk_sync                                   2      14743094   14743094
vx_bc_buf_locksleep(0xe000000130e738f8)            2      14737759   14728259
vx_bc_buf_locksleep(0xe000000130eaab18)            2      14736159   14735259
sigsuspend(0x9fffffff7f7e8000)                     2      14719525   32
vx_bc_buf_locksleep(0xe000000130e12bb8)            2      14718259   14632459
ticks_since_boot                                   2      89         89


Most Common Sleep Callers
=========================

                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
vx_rwsleep_lock()                                  259    14745854   13279485
ksleep()                                           75     3357952451 30
vx_bc_buf_locksleep()                              37     14746559   14623559
waitforpageio()                                    32     14748159   14776
vx_rwsleep_rec_lock()                              32     14746159   14776
semtimedop()                                       20     14740081   44
select()                                           20     3357940821 47
vx_worklist_thread()                               18     159        59
async_daemon()                                     16     14738567   14738567
svc_run()                                          16     14734789   14734789
hpstreams_read_int()                               15     155434794  23710628
ThreadBridge_Sleep()                               12     14748083   14745342
poll()                                             11     1440732218 102
real_nanosleep()                                   8      136726184  2
biowait()                                          8      14748086   14575959
lvmkd_daemon()                                     6      14748558   14748558
pm_sigwait()                                       6      14737626   53
delay()                                            6      8317       49
pm_schedcpu()                                      6      238        38
vx_logflush()                                      5      14745159   14605659
wait1()                                            4      14745854   14625533
fifo_rdwr()                                        4      14736574   14719522
ksync_worker()                                     4      26         1
_lwp_sema_wait()                                   4      3357952451 30335
ken_recv_msg()                                     3      14743094   14743092
lv_attach_daemon()                                 2      14743210   14741987
lv_dev_daemon()                                    2      14741975   14741940
streams_getmsg()                                   2      14735899   14730765
sigsuspend()                                       2      14719525   32
vx_event_wait()                                    2      14698159   59
cv_sleep()                                         2      37953      8729

```

