# System panic with Panic string : Spinlock deadlock!

## System panic with Panic string : Spinlock deadlock!
clock interrupts held off for approx 120 seconds.

```
System crash dump analysis report
=================================
Symptom
-------
  System panic with Panic string : Spinlock deadlock!
  lock name : process lock
  Stack trace of lock holder (cpu 1)

Root Cause
----------
  Processor 1 appears to have had clock interrupts held off for 
  approx 120 seconds. Current SPL = 0xc400000000000000 (SPL6).
  which lead the processor 1 hold the lock too long.
 
  Site environment get over temp frequenty.
  After system reboot, system run well.

Action Plan
-----------
  1. Keep monitor for one month
  2. Site environment improvement as recommendation.

Detail Analysis
---------------
			=======================
			= General Information =
			=======================

Dump time Wed Jun 23 15:29:43 2004 UTC-8
System has been up 5 days, 3 hours, 2 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/A500-7X
HP-UX version    : B.11.11 (64-bit Kernel)
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : PCXW+ (750 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.32  0.16  0.11  

			================
			= Crash Events =
			================


Note: Crash event 0 was a PANIC !
Panic string : Spinlock deadlock!

Lock structure
--------------

0x57849500 lock_t {
0x57849500   uintptr_t sl_lock;                    0x00000000_00000000
0x57849508   uintptr_t sl_owner;                   0x00000000_0131d3c8
0x57849510   char    *sl_name_ptr;                 0x00000000_00944c70
0x57849518   uint16_t sl_flag;                     0x0001
0x5784951a   uint16_t sl_next_cpu;                 0x0000
0x5784951c   uint32_t sl_arena_alloc_flag;         0x9bbc4111
0x57849520 };
lock name : process lock


Stack trace of lock holder (cpu 1)
-----------------------------------

Processor #1
==============  EVENT  ============================
= Event #2 is TOC on CPU #1
= p crash_event_t 0xa81060
= p rpb_t 0x1319370
= Using pc from pim.wide.rp_pcoq_head_hi = 0xc3c6446f
= USER mode!
==============  EVENT  ============================
Can't unwind

Stack trace of lock holder (cpu 1) with all args
-------------------------------------------------

Processor #1
==============  EVENT  ============================
= Event #2 is TOC on CPU #1
= p crash_event_t 0xa81060
= p rpb_t 0x1319370
= Using pc from pim.wide.rp_pcoq_head_hi = 0xc3c6446f
= USER mode!
==============  EVENT  ============================
Can't unwind

Stack Traces for all other Crash events 
=======================================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #0
= p crash_event_t 0xa81000
= p rpb_t 0xa7a100
= Using pc from pim.wide.rp_rp_hi = 0x239d84
==============  EVENT  ============================
SR5=0x0bc46000
                SP         RP Return Name
0x400003ffffff1198 0x00239d84 panic+0x6c
0x400003ffffff10f8 0x001bc9c8 too_much_time+0x2e8
0x400003ffffff1048 0x001bcd84 wait_for_lock+0x23c
0x400003ffffff0ed8 0x00034bfc sl_retry+0x1c
0x400003ffffff0e58 0x0014ff2c syscall+0x2a4
0x400003ffffff0c78 0x00033f5c syscallinit+0x554


==============  EVENT  ============================
= Event #1 is PANIC on CPU #0
= p crash_event_t 0xa81030
= p rpb_t 0xa7a470
= Using pc from pim.wide.rp_rp_hi = 0x239d84
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x0000000000a33bc0 0x00239d84 panic+0x6c
0x0000000000a33b20 0x001bc9c8 too_much_time+0x2e8
0x0000000000a33a70 0x001bcd84 wait_for_lock+0x23c
0x0000000000a33900 0x00034bfc sl_retry+0x1c
0x0000000000a33880 0x000b3b70 sigtimedwait_expire+0x20
0x0000000000a337f0 0x008bde3c invoke_callouts_for_self+0x9c
0x0000000000a33730 0x0003fa88 sw_service+0x100
0x0000000000a33640 0x0016c250 mp_ext_interrupt+0x170
0x0000000000a33520 0x0016aa5c ihandler+0x90c
+-------------  TRAP  ----------------------------
|  Trap type 4 in KERNEL mode at 0x23d7a4 (panic_boot+0x13c)
|  p struct save_state 0.0xa33050
+-------------  TRAP  ----------------------------
SR5=0x0bc46000
                SP         RP Return Name
0x400003ffffff13c8 0x0023d7a4 panic_boot+0x13c
0x400003ffffff1318 0x0023e0b8 boot+0xe0
0x400003ffffff1198 0x00239e6c panic+0x154
0x400003ffffff10f8 0x001bc9c8 too_much_time+0x2e8
0x400003ffffff1048 0x001bcd84 wait_for_lock+0x23c
0x400003ffffff0ed8 0x00034bfc sl_retry+0x1c
0x400003ffffff0e58 0x0014ff2c syscall+0x2a4
0x400003ffffff0c78 0x00033f5c syscallinit+0x554


==============  EVENT  ============================
= Event #2 is TOC on CPU #1
= p crash_event_t 0xa81060
= p rpb_t 0x1319370
= Using pc from pim.wide.rp_pcoq_head_hi = 0xc3c6446f
= USER mode!
==============  EVENT  ============================
Can't unwind


			==================
			= Message Buffer =
			==================

.
.
.
Memory Information:
    physical page size = 4096 bytes, logical page size = 4096 bytes

Spinlock timeout failure:  
 The spinlock code has NOT failed! Instead, some spinlock
 using code has failed to release a spinlock soon enough.
 Address: 0x0000000057849500X ;  owner 0x000000000131d3c8X ;  
 lock 0x0000000000000000X ;  flag 0x1  
 next_cpu 0x0  
 Milliseconds spent spinning =60001
 Millseconds/sec = 1000

		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 2


        s
        t
        a       spin reg eiem/spl         eirr             ipsw            
evt cpu t type  dpth src cr15             cr23             cr22            
--- --- - ----- ---- --- ---------------- ---------------- ----------------
0   0   E PANIC 0    rpb c400000000000000 0800000000002a20 0800001f WRQPDI
1   0   E PANIC 0    rpb c400000000000000 0800000800002a20 0800001b WRQDI
          4          s_s fffffff0ffffffff N/A              0804000b WCQDI
2   1   E TOC   2    rpb c400000000000000 0800040900001548 08000000 W
                     mpi fffffff0ffffffff

Trap Types :
Trap type 4 = External Interrupt (I_EXT_INTP)
Note:  PSW_I bit on processor 1 is off ! (Interrupts are disabled)

WARNING: Processor(s) with PSW_Q bit off !
A processor with the PSW_Q bit off is likely to be running in real mode,
either executing in PDC code or in a low-level trap handler (eg. Data TLB
miss handler). The table below gives either a PDC address, or the Interrupt
Parameter Registers last collected (with the most likely trap type).


cpu trap pcsq.pcoq                     iir        isr.ior                      
--- ---- ----------------------------- ---------- -----------------------------
1   15   0x042f7400.0x00000000c3c6446f 0x43570000 0x00000000.0x0000000000000428

Virtual address information:


VA 0x0.0x428 does not have a translation.
  Hashing details:
  Primary:
    pdirhash=0x02000000=htbl[0]
                              vtag=0x00000000 0x00000000
             hpde2_0_t  pde_next    pde_space  pde_vpage
             0x02000000 0x00000000 0xffffffff 0x00000000


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

hardclock_late = 11
itick_per_tick = 7500000
lbolt          = 44301785 (0x2a3fdd9)

    event mpi                rpb                delta      clk eiem eirr PSW
cpu type  timeinval          interval timer     secs:ticks od  0,4  0,4  I
--- ----- ------------------ ------------------ ---------- --- ---- ---- ---
0   PANIC 0x12e51f5b8ea6c    0x12e51eed9f478         0:15  11  1 0  0 1  1 
1   TOC   0x12e4770372475    0x12e5c7617184d      -120:-39 22  1 0  0 1  0 

Note: For the PANIC crash event type it is normal to see a positive delta ticks.
This is due to the processor executing dumpsys() code after creating it's
crash event and rpb.


WARNING: Processor 1 appears to have had clock interrupts held off for 
approx 120 seconds. Current SPL = 0xc400000000000000 (SPL6).


			=================
			= Load Averages =
			=================


avenrun
=======
0.32  0.16  0.11  

mp_avenrun
==========
cpu0 : 0.319385  0.193175  0.159010  
cpu1 : 0.313927  0.118198  0.070324  

			======================
			= Thread Information =
			======================

0 Threads ran in the last second
0 Threads ran in the last 5 seconds
0 Threads ran in the last 10 seconds
0 Threads ran in the last minute
119 Threads ran in the last hour

statdaemon ran 6044 ticks ago

Note:  statdaemon didn't run in the last second !
The statdaemon should normally run every second.
If it has not run in the past second it may be an indication that
it is being starved by higher priority or looping process(es).


Most Common Wait Channels
=========================
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
per_processor_selects                              29     44289021   6032      
per_processor_selects+0x40                         22     44288975   6026      
vx_worklist_thread_sv                              22     6199       6098      
lvmkd_q                                            6      24421      24421     
streams_blk_sync                                   2      44297143   44297143  
*uptr+0                                            2      17633103   6511      
streams_mp_sync                                    2      6933       6925      
ticks_since_boot                                   2      6044       6044      


Most Common Sleep Callers
=========================
                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
ksleep()                                           101    44288493   6025      
select()                                           41     44289021   6053      
vx_worklist_thread()                               22     6199       6098      
poll()                                             11     44286863   6026      
wait1()                                            9      44286382   17623252  
pm_sigwait()                                       8      44288494   6045      
lvmkd_daemon()                                     6      24421      24421     
hpstreams_read_int()                               5      17628344   17508     
real_nanosleep()                                   5      49815      6025      
thread_wait()                                      3      44288483   44287151  
_lwp_sema_wait()                                   3      17633074   17632996  
fifo_rdwr()                                        2      44287876   43040981  
sosleep()                                          2      44287724   44287176  
hpstreams_read_tty()                               2      43271193   42634845  
sync_buffer_cache()                                2      8054       6421      


Idle Globals
============

candidate_idle_spu = -1
migration_cycles = 1500000

Note:  There are 56 threads with schedpolicy = SCHED_NOAGE!
Look for "*" after the PRI in the thread listings.



Running Threads (TSRUNPROC) and idle Processors
===============================================


                    TICKS      TICKS                     I TICKS
                    SINCE      SINCE                     C SINCE      NREADY
TID     PID   PPID  RUN        IDLE       PRI  SPU STATE S MIGR       FR LO AL COMMAND
------- ----- ----- ---------- ---------- ---- --- ----- - ---------  -- -- -- -------
14730   11232 11196 6025       6025       223* 0   SYS   Y 6032       0  0  0  java 
14727   11232 11196 6025       6026       223* 1   SYS   N 6028       1  0  0  java 

Note: 
FR: free to run on any processor (candidate for thread migration).
LO: locked (via processor affinity/mpctl) to this processor).
AL: Alpha semaphores misses (special scheduling when miss a sema).


Threads waiting on cpu (TSRUN) - sorted by cpu/pri/ticks-since-run
==================================================================

TID     PID   PPID  TICKS      PRI  SPU BND GRP SYSCALL        COMMAND
------- ----- ----- ---------- ---- --- --- --- -------------- -------
12      12    0     44297142   -32  0   N   -1  n/a            ttisr          
556     519   1     11862      154  1   N   -1  select         syslogd        

Note:  There is 1 thread in TSZOMB stat !
```

