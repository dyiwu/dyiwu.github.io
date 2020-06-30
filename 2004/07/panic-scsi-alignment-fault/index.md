# Unaligned Reference Fault in SCSI driver when narrow device connected.

## Unaligned Reference Fault in SCSI driver when narrow device connected.

When the driver finds that the target device is going to the Data Phase
erroneously, it transfers two bytes  at a time and tries to get the
device to an expected phase before aborting the command.  Since narrow
devices transfer only one byte at a time, there is a possibility that
the target device may change phase after transferring one byte resulting
in phase mismatch.  The phase mismatch handler in the driver adjusts the
data address by one which results in an odd byte aligned address.
Later, when this address was  accessed, the system panicked with an
alignment fault.

```
System crash dump analysis report
=================================

Symptom
-------
  System crash with panic string as Data memory protection/access
  rights/alignment fault.

Root Cause
----------
  The Unaligned Reference Fault in C8xx_isrMA() .

Action Plan
-----------
  Please install the following patches:

     PHKL_30510    SCSI IO Cumulative Patch
     PHKL_30511    SCSI Ultra160 Cumulative Patch

Detail Analysis
---------------

			=======================
			= General Information =
			=======================

Dump time Thu Jul  8 16:16:13 2004 UTC-8
System has been up 14 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/785/J5600
HP-UX version    : B.11.11 (64-bit Kernel)
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : PCXW (552 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.11  0.10  0.08  

			================
			= Crash Events =
			================

Panic string : Data memory protection/access rights/alignment fault

Getting trap information from trap marker at 0x0.0x107fbd0
Trap information
================

Type 18: Data Memory Protection Trap/unaligned Data Reference Trap
Interruption Instruction Register:
	IIR = 0x4b810000
Interruption Space and Offset Registers:
	ISR.IOR = 0x0.0xfffffffff4800b59
Interruption Instruction Address Queue:
	PCSQ.PCOQ = 0x0.0x524fdc = c8xx_isrMA+0x3cc
Interrupt Instruction at c8xx_isrMA+0x3cc:
	ldw  0(ret0),r1

Virtual address information:


VA 0x0.0xfffffffff4800b59 translates to PA 0xfffffffff4800b59
  Page table entry: hpde2_0_t 0x269ffe0
    Access rights : 0x10 PDE_AR_KRW
    Protection key: 0x0 KERNEL/PUBLIC
    Page size     : 4KB


Stack Trace for Crash event 0
=============================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #1
= p crash_event_t 0xa02000
= p rpb_t 0x9fb100
= Using pc from pim.wide.rp_rp_hi = 0x226c1c
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x0000000001080290 0x00226c1c panic+0x6c
0x00000000010801f0 0x0027badc report_trap_or_int_and_panic+0x94
0x0000000001080150 0x0027b020 interrupt+0x208
0x00000000010800a0 0x0015bd88 ihandler+0x930
+-------------  TRAP  ----------------------------
|  Trap type 18 in KERNEL mode at 0x524fdc (c8xx_isrMA+0x3cc)
|  p struct save_state 0.0x107fbd0
+-------------  TRAP  ----------------------------
SR4=0x00000000
                SP         RP Return Name
0x000000000107fbd0 0x00524fdc c8xx_isrMA+0x3cc
0x000000000107f9e0 0x00527f70 c8xx_isrSIP+0x158
0x000000000107f8e0 0x00528b0c c8xx_isr+0x334
0x000000000107f670 0x00370784 sapic_interrupt+0x2c
0x000000000107f5e0 0x0015d654 mp_ext_interrupt+0x26c
0x000000000107f4d0 0x0015bd64 ihandler+0x90c
+-------------  TRAP  ----------------------------
|  Trap type 4 in KERNEL mode at 0x16d360 (idle+0x2a8)
|  p struct save_state 0.0x107f000
+-------------  TRAP  ----------------------------
SR4=0x00000000
                SP         RP Return Name
0x0000000006bea2d0 0x0016d360 idle+0x2a8
0x0000000006bea050 0x0016b64c swidle+0x28

Stack Trace for Crash event 0 with all args
===========================================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #1
= p crash_event_t 0xa02000
= p rpb_t 0x9fb100
= Using pc from pim.wide.rp_rp_hi = 0x226c1c
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x0000000001080290 0x00226c1c panic+0x6c
0x00000000010801f0 0x0027badc report_trap_or_int_and_panic+0x94
         arg0: 0x0000000000000002
         arg1: 0x0000000000000012
         arg2: 0x000000000107fbd0
         arg3: 0x00000000008b1d68
0x0000000001080150 0x0027b020 interrupt+0x208
         ....  --------n/a-------
         arg1: 0x000000000107fbd0
0x00000000010800a0 0x0015bd88 ihandler+0x930
+-------------  TRAP  ----------------------------
|  Trap type 18 in KERNEL mode at 0x524fdc (c8xx_isrMA+0x3cc)
|  p struct save_state 0.0x107fbd0
+-------------  TRAP  ----------------------------
SR4=0x00000000
                SP         RP Return Name
0x000000000107fbd0 0x00524fdc c8xx_isrMA+0x3cc
         arg0: 0x0000000041236800
         arg1: 0x0000000043421800
         arg2: 0x0000000043417800
0x000000000107f9e0 0x00527f70 c8xx_isrSIP+0x158
         arg0: 0x0000000041236800
         arg1: 0x0000000043421800
         arg2: 0xffffffffff031600
0x000000000107f8e0 0x00528b0c c8xx_isr+0x334
         arg0: 0x0000000041236800
         arg1: 0x0000000000000000
0x000000000107f670 0x00370784 sapic_interrupt+0x2c
0x000000000107f5e0 0x0015d654 mp_ext_interrupt+0x26c
         arg0: 0x000000000107f000
0x000000000107f4d0 0x0015bd64 ihandler+0x90c
+-------------  TRAP  ----------------------------
|  Trap type 4 in KERNEL mode at 0x16d360 (idle+0x2a8)
|  p struct save_state 0.0x107f000
+-------------  TRAP  ----------------------------
SR4=0x00000000
                SP         RP Return Name
0x0000000006bea2d0 0x0016d360 idle+0x2a8
0x0000000006bea050 0x0016b64c swidle+0x28

System panic with an alignment fault and the following stack trace
when connected to a narrow device:

     panic+0x6c
     report_trap_or_int_and_panic+0x94
     interrupt+0x208
     $ihndlr_rtn+0x0
     c8xx_isrMA+0x3cc
     c8xx_isrSIP+0x158
     c8xx_isr+0x334
     sapic_interrupt+0x2c
     mp_ext_interrupt+0x26c
     ivti_patch_to_nop3+0x0
     idle_nonpset_loop+0x704
     idle+0x7ac
     swidle_exit+0x0

When the driver finds that the target device is going to the Data Phase
erroneously, it transfers two bytes  at a time and tries to get the
device to an expected phase before aborting the command.  Since narrow
devices transfer only one byte at a time, there is a possibility that
the target device may change phase after transferring one byte resulting
in phase mismatch.  The phase mismatch handler in the driver adjusts the
data address by one which results in an odd byte aligned address.
Later, when this address was  accessed, the system panicked with an
alignment fault.
```

