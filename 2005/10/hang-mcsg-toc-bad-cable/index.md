# MC/SG TOC system due to bad cable.

## MC/SG TOC system due to bad cable.

```

System crash dump analysis report
=================================
Symptom
-------
   System down with crash dump.

Root Cause
----------
   All incoming network traffic disapear.
   MC/ServiceGuard unable to maintain contact with cmcld daemon.
   System performing TOC to ensure data integrity.

Action Plan
-----------
  Please verify the cable connection to Hub/Switch at 0/0/0/0

Detail Analysis
---------------

			=======================
			= General Information =
			=======================

Dump time Tue Oct 11 11:17:15 2005 UTC-8
System has been up 14 days, 44 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/L1000-36
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : PCXW (360 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 1.68  3.49  3.85  

			================
			= Crash Events =
			================

Crash event 0 was a TOC !

This is a Service Guard TOC !
Message buffer
==============
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
MC/ServiceGuard: Unable to maintain contact with cmcld daemon.
Performing TOC to ensure data integrity.

Lan cards
=========
Name   PPA   Driver      Interface           Mac       States       IP
              Name      Description        Address    Link IP     Address
--------------------------------------------------------------------------------
lan0    0    btlan3  100Base-TX  Core   0x001083f834ba UP   UP   128.128.1.1  
lan1    1    btlan6  100Base-TX         0x001083f734ba UP   UP   192.9.100.1  
                                                            UP   192.9.100.3  
lan2    2    btlan6  100Base-TX         0x001083f734a3 UP   DOWN 0.0.0.0      

--------------------------------------------------------------------------------
WARNING        : lan0 - It seems that we have not process any incoming frame
                 since 2mn11s
--------------------------------------------------------------------------------
WARNING        : lan1 - It seems that we have not process any incoming frame
                 since 3s
--------------------------------------------------------------------------------
WARNING        : lan2 - It seems that we have not process any incoming frame
                 since 11s
--------------------------------------------------------------------------------


Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x778358
= Using pc from pim.wide.rp_pcoq_head_hi = 0x127f0c
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x0000000002a912a0 0x00127f0c idle+0x3c4
0x0000000002a91050 0x0012b30c swidle+0x20


Stack Traces for other processors 
=================================
Processor #1
==============  EVENT  ============================
= Event #1 is TOC on CPU #1
= p crash_event_t 0x22030
= p rpb_t 0xbe5370
= Using pc from pim.wide.rp_pcoq_head_hi = 0x347fac
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x0000000002a97890 0x00347fac Send_Monarch_TOC+0x54
0x0000000002a97830 0x0044dd68 safety_time_check+0x188
0x0000000002a977a0 0x0012d2dc per_spu_hardclock+0x3c
0x0000000002a976c0 0x0012d908 clock_int+0x58
0x0000000002a975b0 0x0012f660 mp_ext_interrupt+0x150
0x0000000002a974d0 0x00130224 ihandler+0x904
+-------------  TRAP  ----------------------------
|  Trap type 4 in KERNEL mode at 0x128c18 (idle+0x10d0)
|  p struct save_state 0.0x2a97000
+-------------  TRAP  ----------------------------
SR4=0x00000000
                SP         RP Return Name
0x0000000002a942a0 0x00128c18 idle+0x10d0
0x0000000002a94050 0x0012b30c swidle+0x20

		===========================================
		= Message Buffer (in chronological order) =
		===========================================

288 Kbytes, lockable: 420032 Kbytes, available: 448948 Kbytes

    Using 1561 buffers containing 12336 Kbytes of memory.
btlan3: NOTE: MII Link Status Not OK - Check Cable Connection to Hub/Switch at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
btlan3: NOTE: MII Link Status Not OK - Switch Connection to AUI at 0/0/0/0....
btlan3: Reset looper timeout: DMA timeout occurred at 0/0/0/0
btlan3: reset state is 550 at 0/0/0/0....
btlan3: WARNING: AUI Loopback Failed at 0/0/0/0....
MC/ServiceGuard: Unable to maintain contact with cmcld daemon.
Performing TOC to ensure data integrity.

```

