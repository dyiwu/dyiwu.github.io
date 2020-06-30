# System panic with Illegal instruction trap

## System panic with Illegal instruction trap

```

System crash dump analysis report
=================================
Symptom
=======
  System panic with panic string : Illegal instruction trap

Root cause
==========
  Cpu 1 with HPA 0xfffffffffffa2000 didn't check in, and
  the IIR (CR19) from the savestate and the instruction pointed to
  by PCOQ are not consistent !
  IIR = 0xffffffff, PCOQ = 0x34240, *PCOQ = 0x8300400a

Action plan
===========
  Replace CPU 1 at path 162, hpahv fffffffffffa2000

Detail analysis
===============

			=======================
			= General Information =
			=======================

Dump time Thu Jan  1 13:47:40 2004 UTC-8
System has been up 11 hours, 41 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/L2000-5X
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : PCXW+ (540 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.14  0.09  0.08  

			================
			= Crash Events =
			================
Crash event 0 was a PANIC !
Panic string : Illegal instruction trap

Trap information
================

Type 8: Illegal Instruction Trap

Interruption Instruction Register:
	IIR = 0xffffffff

Interruption Instruction Address Queue:
	PCSQ.PCOQ = 0x0.0x34240 = bcmp


The IIR (CR19) from the savestate and the instruction pointed to
by PCOQ are not consistent !
IIR = 0xffffffff, PCOQ = 0x34240, *PCOQ = 0x8300400a

Interrupt Instruction from save_state IIR:
	0xffffffff   ;
Interrupt Instruction at bcmp:
	cmpb,&lt;,n  r0,arg2,0x3424c

Stack Trace for Crash event 0
=============================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #1
= p crash_event_t 0x22000
= p rpb_t 0x7a9e08
= Using pc from pim.wide.rp_rp_hi = 0x319d0c
==============  EVENT  ============================
SR5=0x06655400
                SP         RP Return Name
0x400003ffffff2290 0x00319d0c panic+0x14
0x400003ffffff2210 0x0032dbc8 report_trap_or_int_and_panic+0x80
0x400003ffffff2190 0x00131930 trap+0x958
0x400003ffffff1fc0 0x00130afc thandler+0xd20
+-------------  TRAP  ----------------------------
|  Trap type 8 in KERNEL mode at 0x34240 (bcmp)
|  p struct save_state 0x6655400.0x400003ffffff1af0
+-------------  TRAP  ----------------------------
SR5=0x06655400
                SP         RP Return Name
0x400003ffffff1af0 0x00034240 bcmp
0x400003ffffff1af0 0x0015740c dnlc_search+0xac
0x400003ffffff1a50 0x001571b4 dnlc_lookup+0xa4
0x400003ffffff1990 0x001565b8 vx_fast_lookup+0x48
0x400003ffffff18f0 0x001567f8 vx_lookup+0x90
0x400003ffffff1810 0x0010f344 locallookuppn+0xd4
0x400003ffffff14a0 0x00110288 lookuppn+0xf8
0x400003ffffff13d0 0x000e519c execve+0x1ec
0x400003ffffff0da0 0x00152a70 syscall+0x480
0x400003ffffff0c70 0x00035944 syscallinit+0x54c


		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 4

cpu 1 with HPA 0xfffffffffffa2000 didn't check in !

        s
        t
        a       spin reg eiem/spl         eirr             ipsw            
evt cpu t type  dpth src cr15             cr23             cr22            
--- --- - ----- ---- --- ---------------- ---------------- ----------------
0   1   E PANIC 1    rpb c600000000000000 0000000000000000 0800001f WRQPDI
                     mpi fffffff0ffffffff
          8          s_s c600000000000000 N/A              0804fc1f WCRQPDI
1   0   E TOC   0    rpb fffffff0ffffffff 0000000800000000 0844001f WLCRQPDI
          31         s_s ffffffffffffffff N/A              0004011f CRQPDI
2   2   E TOC   0    rpb fffffff0ffffffff 0000000800000000 084c001f WLBCRQPDI
          31         s_s ffffffffffffffff N/A              0846fc1f WLCVRQPDI
3   3   E TOC   0    rpb fffffff0ffffffff 0000000800000000 0844001f WLCRQPDI
          31         s_s ffffffffffffffff N/A              0846fe1f WLCVRQPDI

Trap Types :

Trap type 8 = Illegal Instruction Trap (I_UNIMPL_INST)
Trap type 31 = Panic Trap (T_PANIC)

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
itick_per_tick = 5400000
lbolt          = 4213311 (0x404a3f)

    event mpi                rpb                delta      clk eiem eirr PSW
cpu type  timeinval          interval timer     secs:ticks od  0,4  0,4  I
--- ----- ------------------ ------------------ ---------- --- ---- ---- ---
0   TOC   0x14d43b339134     0x14d43b1870ab          0:0   0   1 1  0 0  1 
1   PANIC 0x14cc3bb49e59     0x14cc3b0b52c6          0:2   8   1 0  0 0  1 
2   TOC   0x14d43a3ee037     0x14d43a106950          0:0   0   1 1  0 0  1 
3   TOC   0x14d4390d78aa     0x14d438de77ad          0:0   0   1 1  0 0  1 
```

