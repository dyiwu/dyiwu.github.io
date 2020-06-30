# Bus check MCA

## Bus check MCA
Bus Check[0] - A hard fail response was detected in data on the front
side bus (FSB)."

```

System crash dump analysis report
=================================

Symptom
=======
  System abnormal reboot with MCA (Machine Check Abort) bus check.

Root cause
==========
  CPU 0: Bus Check[0] - A hard fail response was detected in data on the front
  side bus (FSB). An hard fail response is typically caused by a failure in 
  the I/O subsystem or the CEC. If the target address is provided it will 
  indicate the target of transaction that received the hard fail response.

  Decoding CPU 0 Bus Check[0] MOD_TARGET_IDENTIFIER as follows,
    MOD_TARGET_IDENTIFIER 0x00000000000b0307 VGA frame buffer

Action plan
===========
 MP (Management Processor) card replacement as recommended.

Detail analysis
===============

			=======================
			= General Information =
			=======================

Dump time Tue May 23 21:25:28 2006 UTC-8
System has been up 15 days, 12 hours, 54 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : server rx2620                   
HP-UX version    : B.11.23
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : IA64 (1299 Mhz)
CPU Architecture : IA-64 0
Load average     : 0.05  0.06  0.05  

			================
			= Crash Events =
			================

Crash event 0 was a MCA !
Note: Created binary file mca_060523.1 of MCA logs

MCA[0]:REVISION:0002
MCA[0]:SEVERITY:0
MCA[0]:Processor Error Device Info decode begins.
MCA[0]:VALIDATION_BITS = 0x000000000100101f
MCA[0]:PSP = 0x28000000fff21130
MCA/CMC[0]: cache check.   info:N/A
MCA/CMC[0]: req:N/A res:N/A
MCA/CMC[0]: tgt:N/A ip:N/A
MCA/CMC[0]: bus check.   info:0x1080000003000141
MCA/CMC[0]: req:N/A res:N/A
MCA/CMC[0]: tgt:0x00000000000b0307 ip:N/A
MCA[0]:PSI_STATIC_STRUCT.VALID_FIELD_BITS=0x000000000000003f
MCA[0]:Processor Error Device Info decode ends.

MCA[0]:Platform Specific Non I/O error
MCA[0]:Platform Specific Data = 0xe0000000010a0500
MCA[0]:Error Status val = 0x0
MCA[0]:Error Status type = NO DATA
MCA/CMC[0]:handler = OS_MCA, sub_type = Generic
MCA/CMC[0]:00 00 00 00 00 00 20 05 e0 00 00 00 00 2f c7 a0 
MCA/CMC[0]:00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
MCA/CMC[0]:00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 


MCA[0]:PCI Mercury Bus Error
MCA[0]:Bus ID = 0xe0
MCA[0]:Error Type = 0x4
MCA[0]:Error Status val = 0x121900
MCA[0]:Error Status type = ERR_TIMEOUT Bus operation time-out.
MCA/CMC[0]:handler = OS_MCA, sub_type = Generic
MCA/CMC[0]:00 00 00 00 00 00 20 04 e0 00 00 00 00 2f c6 90 
MCA/CMC[0]:00 e0 00 04 00 00 00 00 00 00 00 00 fe d2 e0 00 
MCA/CMC[0]:00 00 00 00 00 00 00 00 00 00 00 00 00 0b 03 04 

MCA[0]:mca_wakeup() begins.
MCA[0]:mca_wakeup() proc: 1; semaphore 0xffff000100000000
MCA[0]:mca_wakeup() procs MCA'ed 1, procs rendezvous'ed 1,
 procs INIT'ed 0, procs not rendezvoused 0.
MCA[0]:mca_wakeup() ends.
MCA[1]:Left SAL_MC_RENDEZ.
MCA[1]:made crash_event_table entry.
MCA[1]:mca_rendezvous() ends. 
MCA[0]:MCA occurred.
MCA[0]:Processor State Parameter = 0x28000000fff21130
MCA[0]:uc:0 rc:0 bc:1 tc:0 cc:1 dsize:0
MCA[0]:gr:1 b0:1 b1:1 fp:1 pr:1 br:1 ar:1 rr:1
MCA[0]:tr:1 dr:1 pc:1 cr:1 ex:0 cm:0 rs:1 in:0
MCA[0]:dy:0 pm:0 pi:0 mi:1 tl:0 hd:0 us:0 ci:1
MCA[0]:co:0 sy:0 mn:1 me:1 ra:0 rz:0

```

