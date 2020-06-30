# I-Cache Parity Error on CPU 3 lead spinlock deadlock

## I-Cache Parity Error on CPU 3
Spinlock deadlock failure with LPMC I-cache errors CPU 3 failure.

```

System Crash Dump Analysis Report
=================================

Symptom
-------
 System panic with panic string : Spinlock deadlock

Root Cause
----------
 I-Cache Parity Error on CPU 3.

Action Plan
-----------
 Please replace CPU 3 at path 166, hpa: fffffffffffa6000

Delay Analysis
--------------

p4>hpa2cpu
hpa 0xfffffffffffa0000 : cpu 0
hpa 0xfffffffffffa2000 : cpu 1
hpa 0xfffffffffffa4000 : cpu 2
hpa 0xfffffffffffa6000 : cpu 3

p4>Lpmc
lpmc_count = 90

Last LPMC PIM data :

0x052fd250 struct pim2_0_lpmc_data {
0x052fd250   unsigned int  check_type;             0x80000000
0x052fd254   unsigned int  hversion;               0x00000000
0x052fd258   unsigned int  cache_check;            0x80000000 
                            LPMC type : I-Cache Parity Error. 
0x052fd25c   unsigned int  tlb_check;              0x00000000
0x052fd260   unsigned int  bus_check;              0x00000000
0x052fd264   unsigned int  assists_check;          0x00000000
0x052fd268   unsigned int  assist_state;           0x00000000
0x052fd26c   unsigned int  path_info;              0x00000000
0x052fd270   u_long  sys_resp_addr;                0x00000000_00000000
0x052fd278   u_long  sys_req_addr;                 0x00000000_00000000
0x052fd280 };

LPMC log buffer for I/D-cache parity error :

load diag_lpmc_log_type @lpmc_log max 8
hversion   hpa        pim_size   pim_ptr            
0x00005c40 0xfffa6000 0x000004a0 0x00000000052fd8b0 
0x00005c40 0xfffa6000 0x000004a0 0x00000000052fdd50 
0x00005c40 0xfffa6000 0x000004a0 0x00000000052fe1f0 
0x00005c40 0xfffa6000 0x000004a0 0x00000000052fe690 
0x00005c40 0xfffa6000 0x000004a0 0x00000000052feb30 
0x00005c40 0xfffa6000 0x000004a0 0x00000000052fefd0 
0x00005c40 0xfffa6000 0x000004a0 0x00000000052ff470 
0x00005c40 0xfffa6000 0x000004a0 0x00000000052ff910 

Dump time Mon Apr 12 09:58:01 2004 UTC-8
System has been up 155 days, 4 hours, 48 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/L2000-44
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : PCXW (440 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.62  0.60  0.68  

			================
			= Crash Events =
			================

Panic string : Spinlock deadlock!

Lock structure
--------------

0x00b7b700 lock_t {
0x00b7b700   u_int   sl_lock;                      0x00000000
0x00b7b704   u_int   sl_owner;                     0x00eed858
0x00b7b708   u_int   sl_flag;                      0x00000001
0x00b7b70c   u_int   sl_next_cpu;                  0x00000000
0x00b7b710   u_int   sl_indirect:0:1;              0x0
0x00b7b710   u_int   sl_name_ptr:1:23;             0x1ad4b4
0x00b7b714   u_int   sl_pad[3];                    0x00000000
0x00b7b720 };
lock name : callout_lock

Stack trace of lock holder (cpu 3)
-----------------------------------

Processor #3
==============  EVENT  ============================
= Event #2 is TOC on CPU #3
= p crash_event_t 0x22060
= p rpb_t 0xee3130
= Using pc from pim.wide.rp_pcoq_head_hi = 0xf0f005a3e4 (failed)
= Using pc from pim.wide.rp_rp_hi = 0xf0f005a3cc (failed)
= Using pc from pim.wide.rp_gr31_hi = 0x301030000812204
==============  EVENT  ============================
Can't unwind

Stack trace of lock holder (cpu 3) with all args
-------------------------------------------------

Processor #3
==============  EVENT  ============================
= Event #2 is TOC on CPU #3
= p crash_event_t 0x22060
= p rpb_t 0xee3130
= Using pc from pim.wide.rp_pcoq_head_hi = 0xf0f005a3e4 (failed)
= Using pc from pim.wide.rp_rp_hi = 0xf0f005a3cc (failed)
= Using pc from pim.wide.rp_gr31_hi = 0x301030000812204
==============  EVENT  ============================
Can't unwind

		=======================================
		= Global Error Counters / kmem_writes =
		=======================================

lpmc_count      = 90

Note:  we have LPMC's
Attempting to print lpmc_log data LPMC's

lpmc_log
========

hversion   hpa                cpu pim_size pim_ptr
--------   ------------------ --- -------- -------
0x5c40     0x00000000fffa6000 3   1184     0x52fd8b0         

pim data
========

check_type  :  0x80000000
hversion    :  0x0       
cache_check :  0x80000000
tlb_check   :  0x0       
bus_check   :  0x0       
assts_check :  0x0       
assts_state :  0x0       
path_info   :  0x0       
resp_addr   :  0x0       
req_addr    :  0x0       

		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 4

        s
        t
        a       spin reg eiem/spl         eirr             ipsw            
evt cpu t type  dpth src cr15             cr23             cr22            
--- --- - ----- ---- --- ---------------- ---------------- ----------------
0   0   E PANIC 0    rpb c600000000000000 0800000000000100 0800001b WRQDI
          4          s_s fffffff0ffffffff N/A              0804f81f WCRQPDI
1   1   E TOC   0    rpb c700000000000000 0800000900000080 0804ff08 WCQ
          4          s_s fffffff0ffffffff N/A              0804001f WCRQPDI
2   3   E TOC   2    rpb c600000000000000 0800000100000800 08000008 WQ
                     mpi ffffff00ffffffff
3   2   E TOC   0    rpb c700000000000000 0800000100000400 0804ff08 WCQ
          4          s_s fffffff0ffffffff N/A              0804f61f WCRQPDI

Trap Types :

Trap type 4 = External Interrupt (I_EXT_INTP)
Note:  PSW_I bit on processor 1 is off ! (Interrupts are disabled)
Note:  PSW_I bit on processor 3 is off ! (Interrupts are disabled)
Note:  PSW_I bit on processor 2 is off ! (Interrupts are disabled)

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

hardclock_late = 7
itick_per_tick = 4400000
lbolt          = 1340969121 (0x4fed90a1)

    event mpi                rpb                delta      clk eiem eirr PSW
cpu type  timeinval          interval timer     secs:ticks od  0,4  0,4  I
--- ----- ------------------ ------------------ ---------- --- ---- ---- ---
0   PANIC 0x14f66312d7eb77   0x14f669382ca430      -59:-99 7   1 0  0 1  1 
1   TOC   0x14f663123a6cfd   0x14f66941e3a730      -60:-38 7   1 0  0 1  0 
2   TOC   0x14f663123064ad   0x14f6694197096f      -60:-37 8   1 0  0 1  0 
3   TOC   0x14f663110810ce   0x14f66940f46060      -60:-39 7   1 0  0 1  0 

WARNING: Processor 1 appears to have had clock interrupts held off for 
approx 60 seconds. Current SPL = 0xc700000000000000 (SPL6+CLOCK_RESYNC).

WARNING: Processor 2 appears to have had clock interrupts held off for 
approx 60 seconds. Current SPL = 0xc700000000000000 (SPL6+CLOCK_RESYNC).

WARNING: Processor 3 appears to have had clock interrupts held off for 
approx 60 seconds. Current SPL = 0xc600000000000000 (SPL6).

```

