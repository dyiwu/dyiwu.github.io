# System hang with disk array failure.

## System hang wit disk array failure.

```

System crash dump analysis report
=================================
Symptom
  System hang, INIT dump be taken for cause analysis.

Root Cause
----------
  CISS: RAID SA controller on hardware path 0/4/1/0/4/0 has detected ECC 
  errors in the SIMM modules. Controller be taken offline.
  PV /dev/dsk/c4t0d0s2 is the PV of /dev/vg00, which leads system
  block on IOs and then system hang.

Action Plan
-----------
  Open hardware repair case for ciss PCI-X SmartArray 6402 RAID Controller 
  at address 0/4/1/0/4/0 is recommended.

Detail Analysis
===============

			=======================
			= General Information =
			=======================

Dump time Thu Mar  4 11:05:16 2010 UTC-8
System has been up 37 days, 2 hours, 39 minutes.

Node Name         : localhost
Model             : server rx2620
BIOS revision     : 04.29
HP-UX version     : B.11.23
Kernel whatstring : @(#) $Revision: vmunix:    
                    B11.23_LR FLAVOR=perf Fri Aug 29 22:35:38 PDT 2003 $
Number of CPU's   : 4
Disabled CPU's    : 0
CPU type          : IA64 (1.4 Ghz)
CPU Architecture  : IA-64 0
Load average      : 0.50  0.50  0.51  

			================
			= Crash Events =
			================
Crash event 0 was a INIT !
This seems to be a user initiated INIT !

Stack Trace for crash event 0
=============================


==============  EVENT  ============================
= Event #0 is CT_INIT on CPU #1;
= p crash_event_t 0xe000000100007000
= p rpb_t 0xe0000001013bab80
==============  EVENT  ============================
RR0=0x00ffff31  RR1=0x00ffff31  RR2=0x00ffff31  RR3=0x00ffff31
RR4=0x33bfc831  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0xe0000001013c4000 0xe0000001013d7b90 0xe0000000006ce8c0 idle+0x560
0xe0000001013c4000 0xe0000001013d7bf0 0xe000000000e3d020 os_rendez_ihandler
------------------------------------------------------

0/4/1/0/4/0 ciss PCI-X SmartArray 6402 RAID Controller

CISS: RAID SA controller on hardware path 0/4/1/0/4/0 has detected ECC 
      errors in the SIMM modules. Controller will be taken offline.
CISS: RAID SA driver on hardware path 0/4/1/0/4/0 is being taken offline,
      reason code 0x2
      Reason codes:
	0x01 = Initialization failed.
	0x02 = Firmware crash.
	0x05 = Download timeout.
	0x06 = Firmware initialization timeout.

SCSI: Read error -- dev: b 31 0x040000, errno: 126, resid: 1024,
	blkno: 8, sectno: 1024050, offset: 8192, bcount: 1024.
LVM: VG 64 0x000000: Lost quorum.

This may block configuration changes and I/Os. In order to reestablish 
quorum at least 1 of the following PVs (represented by current link) 
must become available:
<31 0x040002> 

LVM: VG 64 0x000000: PVLink 31 0x040002 Failed! The PV is not accessible.

    Hardware Path: 0/4/1/0/4/0 ( ciss )
    Hardware Path: 0/4/1/0/4/0.0.0 ( sdisk )

--- Volume groups ---
VG Name                     /dev/vg00	
VG Write Access             read/write
VG Status                   available
Max LV                      255
Cur LV                      9
Open LV                     9
Max PV                      16
Cur PV                      1
Act PV                      1
Max PE per PV               4328
VGDA                        2
PE Size (Mbytes)            16
Total PE                    4318
Alloc PE                    4318
Free PE                     0
Total Spare PVs             0
Total Spare PVs in use      0

   --- Physical volumes ---
   PV Name                     /dev/dsk/c4t0d0s2
   PV Status                   unavailable
   Total PE                    4318
   Free PE                     0
   Autoswitch                  On

```

