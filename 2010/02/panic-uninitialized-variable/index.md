# System panic due to uninitialized variable

## System panic due to uninitialized variable

```

System crash dump analysis report
=================================
Symptom
  System panic with panic: Bad News!  
  Bad News: Cannot use the Kernel Stack when interrupted on the ICS.  
  When this behavior occurs, a "panic:  Bad News!" message will be
  generated followed by a stack trace that includes an entry for
  "tcp_rput+0x22a0". 


Root Cause
----------
  The defect (JAGaf71680) is about the kernel panic due to
  an uninitialized variable scid in tcp_rput() function.

    On IPF platform, compiler has been enhanced which causes 
  more speculative loads. For any speculative loads, IPF 
  will leave Nat bit set in register it was trying to load 
  when the load fails. However, since it's speculative load, 
  it does not cause panic immediately and NaT bit is left 
  set. Later in any other functions/subsystems, if compiler 
  allocates un-initialized variable in to the same register 
  where NaT bit was already set by other function/subsystem 
  and is used, it can cause the system crash.

Action Plan
-----------
  This fix is on PHNE_38679, which has been replaced by PHNE_39387 
  Regarding there is a depndency on IP filter product, please install
  the follows.

  B9901AA               A.11.23.17     HP IPFilter 3.5alpha5
  PHCO_38717            1.0            LVM commands patch
  PHKL_31500            1.0            Sept04 base patch
  PHKL_36577            1.0            PM-PSTAT section 2 manpage changes
  PHKL_40487            1.0            LVM Cumulative Patch
  PHNE_37258            1.0            Cumulative STREAMS Patch
  PHNE_38252            1.0            NFS cumulative patch
  PHNE_39387            1.0            cumulative ARPA Transport patch

Detail Analysis
---------------

			=======================
			= General Information =
			=======================

Dump time Sat Feb 27 17:25:16 2010 UTC-8
System has been up 36 days, 5 hours, 49 minutes.

Node Name         : sda1
Model             : localhost
BIOS revision     : 8.22
HP-UX version     : B.11.23
Kernel whatstring : @(#) $Revision: vmunix:    
                    B11.23_LR FLAVOR=perf Fri Aug 29 22:35:38 PDT 2003 $
Number of CPU's   : 16
Disabled CPU's    : 0
CPU type          : IA64 (1.5 Ghz)
CPU Architecture  : IA-64 0
Load average      : 0.01  0.00  0.00  

			================
			= Crash Events =
			================
Panic string : Bad News!

Trap information
================

Getting trap information from save_state_t 0xdead31.0xe000000101be7800
Reason 0x56: NaT Consuption Fault

Interrupt Instruction Pointer:
	IIP = 0xdead31.0xe0000000005df240, slot 0

Interrupt Instruction Bundle:

tcp_rput+0x22a0():  (@ 0xe0000000005df240)
|10756 /ux/core/kern/common/net/netinet/tcp.c
+0x22a0               st8 [r67]=r84
                      nop.m 0x0 
|10758 /ux/core/kern/common/net/netinet/tcp.c
                      br.call.dptk.many b0=0xe00000000055bd40 // tcp_conn_con()
|10756 /ux/core/kern/common/net/netinet/tcp.c
                      ;;

Interrupt Faulting Address:
	IFA = 0xdead31.0xe0000000005df240

Virtual address information:

VA 0xdead31.0xe0000000005df240 translates to PA 0x45df240
  Page table entry: tr_entry_t 0xe0000001018f0580
    Access rights : 0x08 PTE_AR_KRW
    Protection key: 0xbeef KERNEL/PUBLIC
    Page size     : 64MB
    Large page details:
      Addr :            virtual    physical
      Start: 0xe000000000000000  0x04000000
      End  : 0xe000000004000000  0x08000000
PA 0x45df240 is classified as "PC_KCODE" (not dumped):
  Page classification: pgclass_t 0xe000000101daaf5f
Locality for PA 0x45df240 is loc_t 64, loc index 4, loc type ILV.

Stack Trace for Crash event 0
=============================


==============  EVENT  ============================
= Event #0 is CT_PANIC on CPU #10;
= p crash_event_t 0xe000000100007000
= p rpb_t 0xe000000100c2dd50
= Process at 0xe0000001c1b3a000, pid 22682, "sh"
==============  EVENT  ============================
RR0=0x32db1831  RR1=0x165b1831  RR2=0x00008031  RR3=0x00008031
RR4=0x565b1831  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0xe000000101bd4990 0xe000000101be7790 0xe000000001425700 panic+0x380
0xe000000101bd48c0 0xe000000101be77c0 0xe000000000be59c0 bad_news+0x950
0xe000000101bd4898 0xe000000101be77f0 0xe000000000be4780 bubbleup+0x880
+-------------  TRAP  ----------------------------
|  NaT Consuption Fault in KERNEL mode
|    IIP=0xe0000000005df240:0
|    IFA=0xe0000000005df240
|  p struct save_state 0xdead31.0xe000000101be7800
+-------------  TRAP  ----------------------------
0xe000000101bd4688 0xe000000101be7b40 0xe0000000005df240 tcp_rput+0x22a0
0xe000000101bd45a8 0xe000000101be7b90 0xe000000000594130 str_spu_sw_isr+0x1e0
0xe000000101bd4528 0xe000000101be7b90 0xe00000000082fb60 soft_intr_handler+0x220
0xe000000101bd4418 0xe000000101be7bc0 0xe00000000082e2f0 external_interrupt+0x3b0
0xe000000101bd43e8 0xe000000101be7bf0 0xe000000000be4780 bubbleup+0x880
+-------------  TRAP  ----------------------------
|  External Interrupt in KERNEL mode
|    IIP=0xe000000001456240:0
|  p struct save_state 0xdead31.0xe000000101be7c00
+-------------  TRAP  ----------------------------
               BSP                 SP                 IP
0x9fffffff7f7e93d8 0x9fffffff7f7e7b90 0xe000000001456240 spinlock
0x9fffffff7f7e9368 0x9fffffff7f7e7b90 0xe00000000069a2c0 lock_read+0x340
0x9fffffff7f7e92f0 0x9fffffff7f7e7bb0 0xe00000000069a8f0 vfault+0xd0
0x9fffffff7f7e9258 0x9fffffff7f7e7bd0 0xe0000000005591a0 vm_hndlr+0x1e0
0x9fffffff7f7e9230 0x9fffffff7f7e7bf0 0xe000000000be4780 bubbleup+0x880
+-------------  TRAP  ----------------------------
|  Data TLB Fault in USER mode
|    IIP=0x4060750:0
|    IFA=0x200000004002ad58
|  p struct save_state 0x565b1831.0x9fffffff7f7e7c00
+-------------  TRAP  ----------------------------
Note: Trapped in user mode.

```

