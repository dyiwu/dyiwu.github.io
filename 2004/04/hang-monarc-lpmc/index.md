# After LPMC event, monarch processor have had clock interrupts held off.

## Monarch processor have had clock interrupts held off

```

            System crash dump analysis report
            =================================

Symptom
-------
 System behaved as hang. 
 Crash dump be taken by user TOC.

Root Cause
----------
 CPU 0 appear as hang after LPMC event.

Action Plan
-----------
 CPU 0 replacement.

Detail Analysis
---------------

			=======================
			= General Information =
			=======================

Dump time Tue Aug 10 04:15:55 2004 UTC-8
System has been up 44 days, 23 hours, 49 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/N4000-65
HP-UX version    : B.11.11 (64-bit Kernel)
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : PCXW+ (650 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.03  0.04  0.05  

			================
			= Crash Events =
			================


Note: This seems to be a user initiated TOC !
It seems the monarch processor has not updated the system wide clock
for approx 21884 seconds. Concentrate on the stack trace for the monarch
processor (usually CPU 0) !


		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 4


        s
        t
        a       spin reg eiem/spl         eirr             ipsw            
evt cpu t type  dpth src cr15             cr23             cr22            
--- --- - ----- ---- --- ---------------- ---------------- ----------------
0   0   E TOC   1    rpb c400000000000000 8800000000000018 08000008 WQ
                     mpi fffffff0ffffffff
1   1   E TOC   0    rpb fffffff0ffffffff 0000000800000000 0804f91f WCRQPDI
2   2   E TOC   0    rpb fffffff0ffffffff 0000000000000000 082cc81f WNBCRQPDI
3   3   E TOC   0    rpb fffffff0ffffffff 0000000000000000 082ce31f WNBCRQPDI

Note:  PSW_I bit on processor 0 is off ! (Interrupts are disabled)

Outstanding external interrupts
===============================

    eirr
cpu bit  SPL                Handler SPL        Handler
--- ---- --------           -----------        -------
0   0    SPL6/SPINLOCK_EIEM SPL7/PSW_I=0       sampler
0   4    SPL6/SPINLOCK_EIEM SPL6/SPINLOCK_EIEM clock_int
0   59   SPL6/SPINLOCK_EIEM SPL5/SPLIO         sapic_interrupt
0   60   SPL6/SPINLOCK_EIEM SPL5/SPLIO         sapic_interrupt
1   28   SPLNOPREEMPT       SPLNOPREEMPT       take_a_trap

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

hardclock_late = 3312
itick_per_tick = 6500000
lbolt          = 388738234 (0x172bacba)

    event mpi                rpb                delta      clk eiem eirr PSW
cpu type  timeinval          interval timer     secs:ticks od  0,4  0,4  I
--- ----- ------------------ ------------------ ---------- --- ---- ---- ---
0   TOC   0x8fa3c09ba0a23    0x9072c1a22de81    -21884:-93 3312 1 0  1 1  0 
1   TOC   0x9072c19ae49cc    0x9072c19617674         0:0   0   1 1  0 0  1 
2   TOC   0x9072c1850f8c3    0x9072c18042262         0:0   0   1 1  0 0  1 
3   TOC   0x9072c17b4fbfb    0x9072c1768101a         0:0   0   1 1  0 0  1 

Processor 0 appears to have had clock interrupts held off for 
approx 21884 seconds. Current SPL = 0xc400000000000000 (SPL6).

=============

		=======================================
		= Global Error Counters / kmem_writes =
		=======================================

lpmc_count      = 27

lpmc_log
========

hversion   hpa                cpu pim_size pim_ptr
--------   ------------------ --- -------- -------
0x5e80     0xfffffffffed25000 0   1184     0x59268f0         

pim data
========

check_type  :  0x80000000
hversion    :  0x400     
cache_check :  0x80000000
tlb_check   :  0x0       
bus_check   :  0x0       
assts_check :  0x0       
assts_state :  0x0       
path_info   :  0x0       
resp_addr   :  0x0       
req_addr    :  0x0       

hversion   hpa                cpu pim_size pim_ptr
--------   ------------------ --- -------- -------
0x5e80     0xfffffffffed25000 0   1184     0x5926d90         

pim data
========

check_type  :  0x80000000
hversion    :  0x400     
cache_check :  0x80000000
tlb_check   :  0x0       
bus_check   :  0x0       
assts_check :  0x0       
assts_state :  0x0       
path_info   :  0x0       
resp_addr   :  0x0       
req_addr    :  0x0       


libp4 (9.34): Opening ./vmunix ./INDEX

Loading symbols from ./vmunix
Kernel TEXT pages not requested in crashconf
Will use an artificial mapping from a.out TEXT pages

path     hpa              hv  hr sv  sr spa      size   io_hi/lo description
----     ---------------- --- -- --- -- -------- ------ -------- -----------
0        fffffffffed00000 803  0   c  0                          System Bus Adapter (803)
0/0      ffffffffbffe0000 782  0   a  0                          Local PCI Bus Adapter (782)
0/1      ffffffffbffe2000 782  0   a  0                          Local PCI Bus Adapter (782)
0/2      ffffffffbffe4000 782  0   a  0                          Local PCI Bus Adapter (782)
0/4      ffffffffbffe8000 782  0   a  0                          Local PCI Bus Adapter (782)
0/5      ffffffffbffea000 782  0   a  0                          Local PCI Bus Adapter (782)
0/8      ffffffffbfff0000 782  0   a  0                          Local PCI Bus Adapter (782)
0/10     ffffffffbfff4000 782  0   a  0                          Local PCI Bus Adapter (782)
0/12     ffffffffbfff8000 782  0   a  0                          Local PCI Bus Adapter (782)
1        fffffffffed40000 803  0   c  0                          System Bus Adapter (803)
1/0      fffffffffece0000 782  0   a  0                          Local PCI Bus Adapter (782)
1/2      fffffffffece4000 782  0   a  0                          Local PCI Bus Adapter (782)
1/4      fffffffffece8000 782  0   a  0                          Local PCI Bus Adapter (782)
1/8      fffffffffecf0000 782  0   a  0                          Local PCI Bus Adapter (782)
1/10     fffffffffecf4000 782  0   a  0                          Local PCI Bus Adapter (782)
1/12     fffffffffecf8000 782  0   a  0                          Local PCI Bus Adapter (782)
36       fffffffffed24000 584  0   c  0                          Bus Converter
37       fffffffffed25000 5e8  0   4  0                          Processor
44       fffffffffed2c000 584  0   c  0                          Bus Converter
45       fffffffffed2d000 5e8  0   4  0                          Processor
100      fffffffffed64000 584  0   c  0                          Bus Converter
101      fffffffffed65000 5e8  0   4  0                          Processor
108      fffffffffed6c000 584  0   c  0                          Bus Converter
109      fffffffffed6d000 5e8  0   4  0                          Processor
192      fffffffffedc0000  90  0   9  0                          Memory


Fixed Module Table - Number of Fixed Modules =   0
--------------------------------------------------

PCI interface modules - Number of PCI modules =   8
---------------------------------------------------

path              base address reg driver     description
----              ---------------- ------     -----------
0/0/0/0           ffffffff81fbb000 btlan      HP PCI 10/100Base-TX Core
0/0/1/0           ffffffff81fbc000 c720       SCSI C895 Ultra Wide Single-Ended
0/0/2/0           ffffffff80080000 c720       SCSI C87x Ultra Wide Single-Ended
0/0/2/1           ffffffff81fbe000 c720       SCSI C87x Ultra Wide Single-Ended
0/0/4/0           ??               func0      PCI BaseSystem (103c128d)
0/0/4/1           ffffffff80000000 asio0      PCI Serial (103c1048)
0/5/0/0           ffffffff8bfbf000 btlan      HP A5230A/B5509BA PCI 10/100Base-TX Addon
1/12/0/0          ffffffffd9fbf000 btlan      HP A5230A/B5509BA PCI 10/100Base-TX Addon

Physical Memory Address Range(s)
--------------------------------

Start              End             
0x0000000000000000-0x000000007fffffff
0x0000000180000000-0x00000001bfffffff

```

