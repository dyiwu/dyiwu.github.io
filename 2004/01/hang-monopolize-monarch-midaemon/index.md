# System hang due to midaemon is monopolizing the monarch.

## System hang due to midaemon is monopolizing the monarch.

```

System crash dump analysis report
=================================

Symptom
-------
  System behaved as hang, TOC dump be taken for root cause finding.

Root Cause
----------
  Tthe root cause is on the measureware, the mideamon process who was 
  running since more than 5 hours with realtime priority -16 in user space. 

  The mideamon is monopolizing CPU0. All other processes , such as login 
  processes, are still waiting on CPU 0, are mandatory bound to monarch CPU 0. 
  It looks like a defect in modaemon code.

Action Plan
-----------
  Upgrade the measure product.

Detail analysis
---------------
Dump time Thu Jan 29 09:40:52 2004 UTC-8
System has been up 25 days, 17 hours, 50 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/L2000-44
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : PCXW (440 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 88.30  81.10  60.70

Dump time Thu Jan 29 09:40:52 2004 UTC-8
System has been up 25 days, 17 hours, 50 minutes.

Load average             : 88.30  81.10  60.70  
avenrun                  : 88.30  81.10  60.70  
real_run                 : 176.591278  162.192198  121.110764  
pwrun ("fast" io wait)   : 0.000000  0.005075  0.296340  
mp_avenrun cpu0          : 176.519803  162.137169  120.701264  
mp_avenrun cpu1          : 0.071475  0.060104  0.705839  

// CPU 0 is heavy loader

p4> trace processor 0
Cpu 0  : proc[1026] pid=3557 tid=3396  cmd="/opt/perf/bin/midaemon"
Processor #0
==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x749b58
= Using pc from pim.wide.rp_pcoq_head_hi = 0x45ceb
= USER mode!
==============  EVENT  ============================

                    TICKS      TICKS                    I TICKS
                    SINCE      SINCE                    C SINCE      
TID     PID   PPID  RUN        IDLE       PRI SPU STATE S MIGR       COMMAND
------- ----- ----- ---------- ---------- --- --- ----- - ---------  -------
3396    3557  1     212326     212326     -16 0   USER  N 212326     midaemon 


Mideamon is in user space since 2123.26 seconds (212326 ticks)

or to say it is running since more than 5 hours with realtime priority
-16. Currently it is in user space. The cause of the problem is the
mideamon that is monopolizing CPU0. All processes that are still waiting
on CPU 0 are mandatory bound to that CPU. It looks like a defect in
midemon code.

```

