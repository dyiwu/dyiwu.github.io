# MC/SG TOC system due to SCSI I/O time out.

## MC/SG TOC system due to SCSI I/O time out.

```

System crash dump analysis report
=================================

Symptom
-------
  System been brought down by a INIT through the MC/SG safety 
  timer mechanism.

Root Cause
----------
  scsiU320 interface driver is being taken offline for
  SCSI Ultra320 interface at 0/1/1/0 instance 2 and 0/1/1/1
  instance 3, regarding unrecoverable hardware/firmware error.

Action Plan
-----------
  Please upgrade the mpt driver to latest version.
     scsiU320 $Date: Sep 24 2007 $Revision: r11.23/1.04
  The current installed version of mpt driver is 
     scsiU320 $Date: Aug 16 2004 $Revision: r11.23/1.03

Detail analysis
---------------

			=======================
			= General Information =
			=======================

Dump time Sat Apr 11 21:17:38 2009 UTC-8
System has been up 497 days, 3 hours, 48 minutes.

Node Name         : localhost
Model             : server rx4640
BIOS revision     : 03.11
HP-UX version     : B.11.23
Kernel whatstring : @(#) $Revision: vmunix:    
                    B11.23_LR FLAVOR=perf Fri Aug 29 22:35:38 PDT 2003 $
Number of CPU's   : 4
Disabled CPU's    : 0
CPU type          : IA64 (1.5 Ghz)
CPU Architecture  : IA-64 0
Load average      : 0.00  0.00  0.02  


  SCSI I/O time out events occurred on 0/1/1/0 instance 2 
  and 0/1/1/1 instance 3.

  0xe000000100eaa970 Not Written 2009/04/11 20:25:57 
  DIAG_LOG_IO (I/O Error)
  Hardware Path: 0/1/1/1 ( mpt )
    
  0xe000000100eaa950 Not Written 2009/04/11 20:25:57 
  DIAG_LOG_IO (I/O Error)
  Hardware Path: 0/1/1/0 ( mpt )

  SCSI Ultra320 0/1/1/0 instance 2:
    IO Type : SCSI IO has timed-out.  Target ID: 1, LUN ID: 0.
    Read Command - CDB: 28 00 03 1f bb c2 00 00 10 00 
  SCSI Ultra320 0/1/1/0 instance 2:
    An IO timeout condition was detected.
    Condition cleared, no intervention required. 
  SCSI Ultra320 0/1/1/0 instance 2:
    Driver is being taken offline, reason code 3
    Reason codes:           
       1 => Adapter initialization failed.
       2 => Firmware download failed.
       3 => Unrecoverable hardware/firmware error.
       4 => SCSI ID change request failed. 

  SCSI Ultra320 0/1/1/1 instance 3:
    Driver is being taken offline, reason code 3
    Reason codes:           
      1 => Adapter initialization failed.
      2 => Firmware download failed.
      3 => Unrecoverable hardware/firmware error.
      4 => SCSI ID change request failed. 

  which shortly there after led to :
      LVM: VG 64 0x000000: Lost quorum. --> /dev/vg00 lost quorum.

  This may block configuration changes and I/Os.
  In order to reestablish quorum at least 1 of the following PVs
  (represented by current link) must become available:

  <31 0x021002> <31 0x020002> --> /dev/dsk/c2t1d0 /dev/dsk/c2t0d0

  and then,

    MC/ServiceGuard: Unable to maintain contact with cmcld daemon.
    Performing TOC to ensure data integrity.

--- Volume groups ---
VG Name                     /dev/vg00
VG Write Access             read/write
VG Status                   available
Max LV                      255
Cur LV                      4
Open LV                     4
Max PV                      16
Cur PV                      2
Act PV                      2
Max PE per PV               4328
VGDA                        4
PE Size (Mbytes)            16
Total PE                    8636
Alloc PE                    8636
Free PE                     0
Total Spare PVs             0
Total Spare PVs in use      0

  --- Logical volumes ---
.
.
.
  --- Physical volumes ---
  PV Name                     /dev/dsk/c2t1d0
  PV Status                   unavailable
  Total PE                    4318
  Free PE                     0
  Autoswitch                  On

  PV Name                     /dev/dsk/c2t0d0
  PV Status                   unavailable
  Total PE                    4318
  Free PE                     0
  Autoswitch                  On

```

