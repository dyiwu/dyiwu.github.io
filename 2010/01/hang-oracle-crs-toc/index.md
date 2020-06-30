# Oracle CRS initiated INIT on cpu 2.

## Oracle CRS initiated INIT on cpu 2

```

System Crash Dump Analysis Report
=================================

Symptom
-------
 System be INIT (TOC), system crash dump files be taken.
 
Root Cause
----------
 This appears to be an Oracle CRS initiated INIT on cpu 2.

Action Plan
-----------
 Consult Oracle support for this event is recommended.

			=======================
			= General Information =
			=======================

Dump time Wed Jan 20 04:31:03 2010 UTC-8
System has been up 56 days, 15 hours, 30 minutes.

Node Name         : localhost
Model             : server rx8640
BIOS revision     : 7.044
HP-UX version     : B.11.23
Kernel whatstring : @(#) $Revision: vmunix:    
                    B11.23_LR FLAVOR=perf Fri Aug 29 22:35:38 PDT 2003 $
Number of CPU's   : 24
CPU type          : IA64 (1.6 Ghz)
CPU Architecture  : IA-64 0
Load average      : 0.01  0.01  0.01  

			================
			= Crash Events =
			================

Note: Crash event 0 was a INIT !

Note: This appears to be an Oracle CRS initiated INIT on cpu 2 !

There are 2 "init.cssd daemon" processes (should have only one) !
Unexpected node reboot on death of "init.cssd fatal"
Please contact Oracle Support for the appropriate resolution.

Process list of init.cssd as below,

There are 2 "init.cssd daemon" processes (should have only one) !
Unexpected node reboot on death of "init.cssd fatal"
Process list of init.cssd as below,
1425207 1315  1250  1         158  7   SLEEP waitpid        init.cssd
                                             wait1(0xe0000001ebc8c000)
1425106 1250  1     42        158  20  SLEEP waitpid        init.cssd
                                             wait1(0xe00000019541e000)
1425204 1312  1250  252       158  4   SLEEP waitpid        init.cssd
                                             wait1(0xe0000001c3a96000)
7688    5673  1     489384668 158  2   SLEEP waitpid        init.cssd
                                             wait1(0xe0000001c2e16000)
7684    5669  1     489386507 158  16  SLEEP waitpid        init.cssd
                                             wait1(0xe00000019545e000)

```

