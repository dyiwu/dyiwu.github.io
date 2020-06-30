# Lock contention leads system hang.

On a multiprocessor system, vhand will consume 100% of a
cpu in system mode as seen using a performance monitoring
tool.  This has been observed in a system under high
memory pressure while starting up a large database application.

```

System crash dump analysis report
=================================
Symptom
-------
 System behaved as hang, TOC dump be taken for root cause finding.

Root Cause
----------
 A contention between beta semaphore shm_lock and vmtotal_lock. 
 This lead to hang situation.

Action Plan
-----------
  Cumulative VM patch PHKL_36133 installation is recommended. 
  PHKL_36133 (2) [S] Cumulative VM, Psets, Preemption, PRM, MRG 
  (JAGae20932)

Detail Analysis
---------------
p4> SystemInfo
System Name      :  HP-UX
Node Name        :  localhost
Model            :  9000/800/SD64000
HP-UX version    :  B.11.11 (64-bit Kernel)
Number of CPUs   :  8
Disabled  CPUs   :  2
CPU type         :  PCX-W+ (875 Mhz)
CPU Architecture :  PA-RISC 2.0
Physical Memory  :  2093056 pages ( 7.98 Gb )

Panic string     :    , isr.ior = 0'0.0'0
Boot string      :  disk(12/0/8/0/0.0.0.0.0.0.0;0)/stand/vmunix
Kernel KRS file  :  /stand/krs/_stand_vmunix.krs
System KRS file  :  /stand/krs/system.krs
System boot time :  Tue Apr  8 17:23:31 2008 (TZ=UTC-8)
System down time :  Tue Nov 11 04:50:36 2008 (TZ=UTC-8)
System was up for 1870431197 ticks ( 216 days 11 hours 38 mins 31 secs )

Load averages:
avenrun          :  2.000  2.000  2.000
real_run         :  2.000  2.000  2.000
disk wait jobs   :  22  [hint of 'pwrun' from vmtotal() - 1653648 ticks ago]
mp_avenrun[ 0]   :  0.000  0.000  0.000
mp_avenrun[ 1]   :  0.000  0.000  0.000
mp_avenrun[ 2]   :  0.000  0.000  0.000
mp_avenrun[ 3]*  :  0.000  0.000  0.000
mp_avenrun[ 4]   :  0.000  0.000  0.000
mp_avenrun[ 5]   :  2.000  2.000  2.000
mp_avenrun[ 6]*  :  0.048  0.015  0.005
mp_avenrun[ 7]   :  0.000  0.000  0.000
           ( * =>  disabled cpu )
deactivated cnt  :  20

Kthread states:
Number of threads in TSRUN   state = 23
Number of threads in TSSLEEP state = 431
Number of threads in TSSTOP  state = 10
Number of threads in TSZOMB  state = 5
Number of threads in TSIDL   state = 6
Number of threads with TSDEACT flag = 21
Number of threads with TSDEACTSELF flag = 1

Processor states:
Processor :  Cpu_State          Operational_State  Config_State
cpu[  0]  :  SPUSTATE_IDLE      SPU_ENABLED        PROC_CONFIGED
cpu[  1]  :  SPUSTATE_IDLE      SPU_ENABLED        PROC_CONFIGED
cpu[  2]  :  SPUSTATE_IDLE      SPU_ENABLED        PROC_CONFIGED
cpu[  3]  :  SPUSTATE_IDLE      SPU_DISABLED       PROC_CONFIGED
cpu[  4]  :  SPUSTATE_IDLE      SPU_ENABLED        PROC_CONFIGED
cpu[  5]  :  SPUSTATE_SYSTEM    SPU_ENABLED        PROC_CONFIGED
cpu[  6]  :  SPUSTATE_IDLE      SPU_DISABLED       PROC_CONFIGED
cpu[  7]  :  SPUSTATE_IDLE      SPU_ENABLED        PROC_CONFIGED

The load averages is very low and most of the processors (except cpu 5)
are idle. The processors are healthy and they are not in a blocked state.

Crashinfo output clearly shows that there is a contention for the beta
semaphore shm_lock and vmtotal_lock. This is not good since this will lead
to hang situation.

Most Common Wait Channels
=========================
                                                         ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
shm_lock                                           69     1652651    1473696
vx_worklist_thread_sv                              25     128        28
async_bufhead                                      20     1870420709 1870420709
lock_read(0x56c14ed8)                              18     1726014    1602477
voliod_sync                                        10     1870425045 1870425042
per_processor_selects                              8      1870419272 4
streams_mp_sync                                    8      9620508    9620482
memory_sleepers                                    8      1473679    52665
per_processor_selects+0x80                         7      1870415140 1180
vmtotal_lock                                       7      1629625    1479673
...

And their waiters are ...
Threads waiting on Beta Semaphores
==================================

               TICKS
WAITING OWNER   SINCE
TID     TID     RUN        CPU PRI SEMA ADDRESS   CALLER
------- ------- ---------- --- --- -------------- ----------
133139  312772  1629625    0   646 0xef3148       vmtotal+0x38
3930    312772  1623646    0   571 0xef3148       vmtotal+0x38
133203  312772  1599623    0   646 0xef3148       vmtotal+0x38
133268  312772  1569617    0   646 0xef3148       vmtotal+0x38
133338  312772  1539596    0   646 0xef3148       vmtotal+0x38
133403  312772  1509605    0   646 0xef3148       vmtotal+0x38
133486  312772  1479673    0   646 0xef3148       vmtotal+0x38
133080  133079  1652651    0   646 0xeba700       shmget+0x44
312772  133079  1649495    0   646 0xeba700       shm_unattached_vmtotal+0x24
5367    133079  1644634    0   646 0xeba700       shm_fork+0x20
133087  133079  1641657    0   646 0xeba700       shmget+0x44
133145  133079  1623653    0   646 0xeba700       shmget+0x44
133151  133079  1611649    0   646 0xeba700       shmget+0x44
...

Let's see what these two owner threads (tid 312772 and 133079) are doing;

p4> trace -U tid 312772
proc_list{131} pid=24466 tid=312772  cmd="ora_pmon_ARDR"
p_stat="SINUSE" : kt_stat="TSSLEEP"
Process  : p proc_t    0x569cd040
          pid=24466 oracle
Kthread  : p kthread_t 0x5c08d040
Using PCB: p user_t 0x8619c00.0x400003ffffff0000
SR5=0x8619c00
LVL  FUNC   ( ARG0, ARG1, ARG2, ARG3, ARG4 )
0)  _swtch+0xc4   ( 0x1ec82e, n/a, n/a, n/a, n/a )
1)  _mp_b_sema_sleep+0x108   ( 0xeba700, n/a, n/a, n/a, n/a )
2)  shm_unattached_vmtotal+0x24   ( 0x400003ffffff0f94, 0x400003ffffff0f98, n/a, n/a, n/a )
3)  vmtotal+0x110   ( n/a, n/a, n/a, n/a, 0xffffffffffffffff )
4)  pstat_dynamic+0xa4   ( n/a, n/a, n/a, n/a, n/a )
5)  pstat+0xf4   ( 0x400003ffffff03a0, 0x8d5aec, n/a, 0xb25510, n/a )
6)  syscall+0xaec   ( n/a, n/a, n/a, n/a, n/a )
7)  syscallinit+0x554   ( n/a, n/a, n/a, n/a, n/a )

p4> trace -U tid 133079
proc_list{202} pid=28294 tid=133079  cmd="oracleARDR 
    (DESCRIPTION=(LOCAL=YES)(ADDR" p_stat="SINUSE" : kt_stat="TSSLEEP"
Process  : p proc_t    0x782a0040
          pid=28294 oracle
Kthread  : p kthread_t 0x67d53040
Using PCB: p user_t 0x1d3d000.0x400003ffffff0000
SR5=0x1d3d000
LVL  FUNC   ( ARG0, ARG1, ARG2, ARG3 )
0)  _swtch+0xc4   ( 0x1634c4, n/a, n/a, n/a )
1)  _sleep_one+0x1a8   ( 0x56c14ede, 0x285, n/a, n/a )
2)  lock_write+0x33c   ( 0x56c14ec8, n/a, n/a, n/a )
3)  shmat+0x184   ( 0x400003ffffff03a0, 0x8d55f8, n/a, 0xb25510 )
4)  syscall+0xaec   ( n/a, n/a, n/a, n/a )
5)  syscallinit+0x554   ( n/a, n/a, n/a, n/a )

Tid 312772 got the vmtotal_lock bsema and is now asleep waiting for
shm_lock. So, all the beta semaphores waiters are due to tid 133079 which
is currently holding shm_lock. This thread has gone to sleep waiting for a
rw_lock. 

p4> Kthread -t 312772
Loaded 1 kthread_t entries in 'DefaultView'
p4> pview -V
   Kthread     tid  pri  spu grp bind  kt_stat    lastrun    pid  comm       syscall kt_wchan
0x5c08d040  312772  134    1  -1   no  TSSLEEP    1649495  24466  oracle     pstat   0

p4> pview -V
   Kthread     tid  pri  spu grp bind  kt_stat    lastrun    pid  comm       syscall kt_wchan
0x67d53040  133079  133    2  -1   no  TSSLEEP    1653533  28294  oracle     shmat 0x56c14ede


p4> Kthread -t 2
Loaded 1 kthread_t entries in 'DefaultView'
p4> pview -V
   Kthread     tid  pri  spu grp bind  kt_stat    lastrun    pid  comm       syscall kt_wchan
0x515a8040       2  128    5  -1   no    TSRUN    1726013      2  vhand      KI_vHAND 0

The owner of this rw_lock is responsible for tid 133079 being asleep for so
long is. The rw_lock structure shows who its owner is ie,

p4> p struct rw_lock 0x56c14ec8
0x56c14ec8
0x56c14ec8 :
0x56c14ec8 struct rw_lock {
0x56c14ec8   lock_t  *interlock;                   0x00000000_1a98f7c0
0x56c14ed0   unsigned int  delay;                  0x00000000
0x56c14ed4   unsigned int  write_waiters;          0x00000002
0x56c14ed8   int     read_count;                   0x00000000
0x56c14edc   char    want_write;                   0x01
0x56c14edd   char    want_upgrade;                 0x00
0x56c14ede   char    waiting;                      0x09
0x56c14edf   char    rwl_flags;                    0x05
0x56c14ee0   struct kthread *l_kthread;            0x00000000_515a8040
0x56c14ee8 };

p4> trace -U -T 0x00000000_515a8040

proc_list{5} pid=2 tid=2  cmd="vhand"
p_stat="SINUSE" : kt_stat="TSRUN"
Process  : p proc_t    0x515a7040
          pid=2 vhand
Kthread  : p kthread_t 0x515a8040
          Running on SPU #5

Processor #5
==============  EVENT  ============================
= Event #7 is TOC on CPU #5
= p crash_event_t 0xa79150
= p rpb_t 0x37b8ef0
= Using pc from pim.wide.rp_pcoq_head_hi = 0x176b34
==============  EVENT  ============================
SR5=0x1f04800
LVL  FUNC   ( ARG0, ARG1, ARG2, ARG3, ARG4, ARG5, ARG6 )
0)  superpage_unlock+0x84   ( 0x5ab67, n/a, n/a, n/a, n/a, n/a, n/a )
1)  vhand_vfdcheck+0x110   ( 0x5ab67, 0x453b9938, 0x453b993c, 0x400003ffffff0c70, n/a, n/a, n/a )
2)  for_val3+0x5c   ( 0x56c14e40, n/a, n/a, n/a, 0x400003ffffff0d10, n/a, n/a )
3)  for_val2+0x29c   ( 0x575c21c0, n/a, 0x40000, n/a, 0x471f30, 0x400003ffffff0d10, 0x4 )
4)  for_val2+0x13c   ( 0x575c21c0, n/a, 0x40000, n/a, 0x471f30, 0x400003ffffff0d10, 0x3 )
5)  for_val2+0x13c   ( 0x575c21c0, n/a, 0x40000, n/a, 0x471f30, 0x400003ffffff0d10, 0x2 )
6)  for_val2+0x13c   ( n/a, n/a, n/a, n/a, 0x471f30, 0x400003ffffff0d10, 0x1 )
7)  foreach_valid+0x54   ( 0x56c14e40, 0x40000, 0x40000, 0x18f640, 0x400003ffffff0c70, n/a, n/a )
8)  agepages+0x11c   ( 0x56649440, 0xef3, n/a, n/a, 0xbd79e0, n/a, n/a )
9)  vhand_core+0x294   ( 0x400003ffffff0a50, 0x0, 0x0, n/a, n/a, n/a, n/a )
10)  vhand+0x2fc   ( n/a, n/a, n/a, n/a, n/a, n/a, n/a )
11)  im_vhand+0xd4   ( n/a, n/a, n/a, n/a, n/a, n/a, n/a )
12)  DoCalllist+0x3c   ( n/a, 0x1, n/a, n/a, n/a, n/a, n/a )
13)  main+0x28   ( n/a, n/a, n/a, n/a, n/a, n/a, n/a )
14)  $vstart+0x48   ( n/a, n/a, n/a, n/a, n/a, n/a, n/a )
15)  $locore+0x94   ( n/a, n/a, n/a, n/a, n/a, n/a, n/a )

Vhand owns the rw_lock and is currently running on cpu 5. It has been
hogging cpu 5 for a long time ie,

p4> ProcInfo 5
Cpu state and thread information :
----------------------------------
cpu                hpa    uareasid      threadp  prevthreadp curstate
 5 0xfffffffffcc7a000   0x1f04800   0x515a8040   0x515a8040 SPUSTATE_SYSTEM

ICS, idle and kernel stack information :
----------------------------------------
cpu          ics        ibase       topics    idlestack          ksp
 5    0x3806000    0x3806000    0x3811000   0x1a9af000            0

Last idle, last ran a timeshare thread, last entry into kernel :
-----------------=----------------------------------------------
ticks_since_boot = 0x6f7c83dd
cpu  last_idletime        last_tsharetime      preempt_start_time
 5  0x6f622da0 (1726013) 0x6f622da0 (1726013) 0x6f622e0b (1725906)

This problem has been resolved by patch PHKL_27825. Its latest replacement
patch is PHKL_36133. I would suggest that the customer install PHKL_36133
to address this problem.

```

