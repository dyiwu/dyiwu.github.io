# I-cache errors lead spinlock deadlock

## I-cache errors lead spinlock deadlock

```

System crash dump analysis report
=================================
Symptom
=======
  System panic with panic string : Spinlock deadlock!
  Lock name : sched_lock
  Lock holder cpu 1

Root cause
==========
  Spinlock deadlock failure with LPMC I-cache errors CPU 1 failure.

Action plan
===========
  Replace CPU 1 at path 45, hpahv fffffffffed2d000

Detail analysis
===============
			=======================
			= General Information =
			=======================
Dump time Mon Aug  1 21:29:34 2005 UTC-8
System has been up 94 days, 9 hours, 27 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/N4000-75
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : PCXW+ (750 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.03  0.08  0.22  

			================
			= Crash Events =
			================
Panic string : Spinlock deadlock!

Lock structure
--------------
0x00df55c0 lock_t {
0x00df55c0   u_int   sl_lock;                      0x00000000
0x00df55c4   u_int   sl_owner;                     0x015a42c8
0x00df55c8   u_int   sl_flag;                      0x00000001
0x00df55cc   u_int   sl_next_cpu;                  0x00000000
0x00df55d0   u_int   sl_indirect:0:1;              0x0
0x00df55d0   u_int   sl_name_ptr:1:23;             0x1a7ab0
0x00df55d4   u_int   sl_pad[3];                    0x00000000
0x00df55e0 };
lock name : sched_lock

Stack trace of lock holder (cpu 1)
-----------------------------------
Processor #1
==============  EVENT  ============================
= Event #4 is TOC on CPU #1
= p crash_event_t 0x220c0
= p rpb_t 0x159f370
= Using pc from pim.wide.rp_pcoq_head_hi = 0xffff956468 (failed)
= Using pc from pim.wide.rp_rp_hi = 0xffff956448 (failed)
= Using pc from pim.wide.rp_gr31_hi = 0x203030c00812204
==============  EVENT  ============================

Cpu 1  : proc[59] pid=1802 tid=1876  cmd="/opt/perf/bin/midaemon"
SR4=0x0
        SP     SZ        RP  FUNC
0x188c4db0   0x70  0x317774  prf_insert_line+0xac
0x188c4d40   0x90  0x317558  prf_klog_putchar+0xa0
0x188c4cb0   0x90  0x318e80  prf_putchar+0xb8
0x188c4c20  0x420   0x8f744  prf+0xdc4
0x188c4800   0x80  0x318cbc  msg_printf+0x6c
0x188c4780  0x220  0x337518  lpmc+0x120
0x188c4560   0x90  0x32af20  interrupt+0x240
0x188c44d0  0x4d0  0x130784  ihandler+0x904
+-------------  TRAP  ----------------------------
|  Trap type 4 in KERNEL mode at 0x1d3430 (cycles_to_ticks+0x70)
|  p struct save_state 0.0x188c4000
+-------------  TRAP  ----------------------------
SR5=0xe3ad000
                SP     SZ        RP  FUNC
0x400003ffffff11c0   0x40  0x1d3430  cycles_to_ticks+0x70
0x400003ffffff1180  0x270  0x1d3018  pstat_proc_fillin+0x668
0x400003ffffff0f10  0x110   0xbe044  pstat_proc+0x164
0x400003ffffff0e00   0x60   0x8b6b8  pstat+0x60
0x400003ffffff0da0  0x130  0x157638  syscall+0x480
0x400003ffffff0c70  0x4d0   0x35944  syscallinit+0x54c

LPMC type : I-Cache Parity Error.
LPMC type : I-Cache Parity Error.
Spinlock timeout failure:

The spinlock code has NOT failed! Instead, some spinlock
using code has failed to release a spinlock soon enough.
Address: 0x0000000000df55c0X ;  owner 0x15A42C8 ;  lock 0x0 ;  flag 0x1
next_cpu 0x0
Milliseconds spent spinning =60001
Millseconds/sec = 1000


lpmc_log
========
lpmc_count      = 2

hversion   hpa                cpu pim_size pim_ptr
--------   ------------------ --- -------- -------
0x5e80     0x00000000fed2d000 1   1184     0x95bc8b0         

pim data
========
check_type  :  0x80000000
hversion    :  0x80      
cache_check :  0x80000000
tlb_check   :  0x0       
bus_check   :  0x0       
assts_check :  0x0       
assts_state :  0x0       
path_info   :  0x0       
resp_addr   :  0x0       
req_addr    :  0x0       

```

