# Data page fault panicon tcp_wput()

## Data page fault panic. 
A null pointer de-reference on tcp_wput() routine.

```

System crash dump analysis report
=================================
Symptom
-------
System panic with
   Panic string : Data page fault
   Eent #0  : proc_list{359} pid=7623 tid=1213865
              cmd="/opt/omni/lbin/vbda -bmaname L"
   Stack trace :
    ==============  EVENT  ============================
    FUNC
    panic+0x6c
    report_trap_or_int_and_panic+0x94
    trap+0xefc
    thandler+0xd20
    +-------------  TRAP  ----------------------------
    |  Trap type 15 in KERNEL mode at 0x14be40 (tcp_wput+0x48)
    |  p struct save_state 0x9af000.0x400003ffffff1708
    +-------------  TRAP  ----------------------------
    FUNC
    tcp_wput+0x48   // panic here
    putnext+0xcc
    streams_write_uio+0x210
    sosend+0x1674
    soo_rw+0x80
    write+0x108
    syscall+0x750
    syscallinit+0x55c


Root Cause
----------
  A null pointer de-reference on tcp_wput() routine.

Action Plan
-----------
 Please apply the ARPA Transport patch and dependencies.  (14 patches 5680KB)

PHNE_33159  cumulative ARPA Transport patch
PHKL_25652  thread nostop for NFS, rlimit max value fix
PHKL_27096  VxVM,EMC,Psets,vPar,slpq1,earlyKRS
PHKL_28122  signals,threads enhancement,Psets Enablement
PHKL_29696  data page fault, use new STREAMS functions
PHKL_29704  Psets Enablement, SCHED_NOAGE, FSS
PHKL_30034  Psets Enablement Patch; FSS cap
PHKL_30035  Psets Enablement; FSS iCOD; callback; FSS
PHKL_30373  select(2) delay/hang and poll(2) hang
PHKL_30516  Buffercache performance; select_enh tunable
PHKL_31993  Core PM, vPar, Psets, slpq1, FSS, rtprio
PHKL_32061  detach;NOSTOP,Abrt,Psets;slpq1;FSS;getlwp
PHKL_33408  RTSCHED performance enablement patch
PHNE_33729  Cumulative STREAMS Patch


Detail Analysis
---------------

Dump time Sun Apr  9 02:30:57 2006 UTC-8
System has been up 20 days, 15 hours, 43 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/K580
HP-UX version    : B.11.11 (64-bit Kernel)
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : PCXU (240 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.30  0.29  0.19  

A null pointer de-reference on tcp_wput() routine.

Type 15: Data TLB Miss Fault/Data Page Fault

Interruption Instruction Register:
	IIR = 0x40370022

Interruption Space and Offset Registers:
	ISR.IOR = 0x0.0x11   // ---- ISR value is incorrect

Interruption Instruction Address Queue:
	PCSQ.PCOQ = 0x0.0x14be40 = tcp_wput+0x48

Interrupt Instruction at tcp_wput+0x48:
	ldb  0x11(r1),arg3

Stack Trace for Crash event 0 with all args
===========================================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #3
= p crash_event_t 0xaaa000
= p rpb_t 0xaa3100
= Using pc from pim.wide.rp_rp_hi = 0x214ca4
==============  EVENT  ============================
SR5=0x009af000
                SP         RP Return Name
0x400003ffffff1f08 0x00214ca4 panic+0x6c
0x400003ffffff1e68 0x00269d54 report_trap_or_int_and_panic+0x94
         arg0: 0x0000000000000001
         arg1: 0x000000000000000f
         arg2: 0x400003ffffff1708
         arg3: 0x000000000094ad48
0x400003ffffff1dc8 0x0016a5d4 trap+0xefc
         ....  --------n/a-------
         arg1: 0x400003ffffff1708
0x400003ffffff1bd8 0x0016c8dc thandler+0xd20
+-------------  TRAP  ----------------------------
|  Trap type 15 in KERNEL mode at 0x14be40 (tcp_wput+0x48)
|  p struct save_state 0x9af000.0x400003ffffff1708
+-------------  TRAP  ----------------------------
SR5=0x009af000
                SP         RP Return Name
0x400003ffffff1708 0x0014be40 tcp_wput+0x48
         arg0: 0x000000004a3c3d80
         arg1: 0x000000004f772080
0x400003ffffff1508 0x0014ede4 putnext+0xcc
         arg0: 0x0000000056b875c0
         arg1: 0x000000004f772080
0x400003ffffff1428 0x0014f168 streams_write_uio+0x210
         arg0: 0x0000000056fe5400
         arg1: 0x400003ffffff0de8
         ....  --------n/a-------
         arg3: 0x400003ffffff1078
0x400003ffffff11f8 0x001536b4 sosend+0x1674
         ....  --------n/a-------
         arg1: 0x0000000000000000
         ....  --------n/a-------
         arg3: 0x0000000000000000
         arg4: 0x0000000000000000
         arg5: 0x0000000000000000
         arg6: 0x0000000000000000
0x400003ffffff0f68 0x004705a0 soo_rw+0x80
         arg0: 0x000000000716cc60
         arg1: 0x0000000000000002
         arg2: 0x400003ffffff0de8
         ....  --------n/a-------
         arg6: 0x0000000000470520
0x400003ffffff0ed8 0x00128d20 write+0x108
         arg0: 0x400003ffffff03a0
         ....  --------n/a-------
         arg2: 0x0000000000903840
0x400003ffffff0dd8 0x00150918 syscall+0x750
0x400003ffffff0c78 0x00033f64 syscallinit+0x55c

```

