# System panic due to 2 bits flipped on CPU

## System panic due to 2 bits flipped on CPU

```

System crash dump analysis report
=================================
Symptom
-------
  System panic in bbds_trim_size() with data TLB fault in kernel mode.

Root Cause
----------
  2 bits flipped on CPU #39 which leads null pointer dereference
  in bbds_trim_size().

Action Plan
-----------
 CPU #39 replacement at hardware path 2/127.

Detail Analysis
===============
Dump time Thu Apr 29 23:41:02 2010 UTC0
System has been up 8 days, 2 hours, 33 minutes.

Node Name         : localhost
Model             : server rx8640
BIOS revision     : 9.048
HP-UX version     : B.11.31
Kernel whatstring : @(#) $Revision: vmunix:    B.11.31_LR FLAVOR=perf
Number of CPU's   : 64
Disabled CPU's    : 0
CPU Architecture  : IA64
CPU type          : Montvale (1.6 Ghz)
Hyper-Threading   : Supported, Enabled in hardware, Enabled in kernel
Load average      : 0.06  0.05  0.05


Event #0  : proc_list{347} pid=14468 tid=2612466  
                     cmd="/etc/opt/resmon/lbin/registrar"
==============  EVENT  ============================
= Event #0 is CT_PANIC on CPU #39;
= p crash_event_t 0xe000000100391000
= p rpb_t 0xe000000100c10290
= Process at 0xe000000937de9300, pid 14468, "registrar"
==============  EVENT  ============================
RR0=0x4f0d3031  RR1=0x767c1031  RR2=0x10010031  RR3=0x10010031
RR4=0x345c1031  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                  SP  FUNC  ( IN0, IN1 )
0x9fffffff7f7e95f0  0x9fffffff7f7e75d0  panic+0x3f0   
                          ( 0xe000000000555cb0, 0x144800206c61015b )
               v--                 v--  post_hndlr(inlined)
0x9fffffff7f7e9528  0x9fffffff7f7e75e0  $cold_vm_hndlr+0x940   
                          ( 0x9fffffff7f7e7600, 0x9fffffff7f7e77d0 )
0x9fffffff7f7e9500  0x9fffffff7f7e75f0  bubbleup+0x880   ( )
+-------------  TRAP #1  ----------------------------
|  Data TLB Fault in KERNEL mode
|    IIP=0xe000000000e27ad0:0
|    IFA=0x40000006384ef220
|  p struct save_state 0x345c1031.0x9fffffff7f7e7600
+-------------  TRAP #1  ----------------------------
               BSP                  SP  FUNC  ( IN0, IN1, IN2 )
0x9fffffff7f7e9458  0x9fffffff7f7e78d0  bbds_trim_size+0x2d0   
                        ( 0xe00000073511c500, 0x2, 0xe000000937de9480 )
0x9fffffff7f7e9378  0x9fffffff7f7e78d0  reapfds+0x130   
                        ( 0xe000000937de9300, 0xe000000937de9480 )
0x9fffffff7f7e9310  0x9fffffff7f7e78d0  exec_cleanup+0x3b0   ( )
0x9fffffff7f7e9230  0x9fffffff7f7e78d0  execve+0xa30   ( 0x9fffffff7f7e8d00 )
0x9fffffff7f7e9150  0x9fffffff7f7e7be0  syscall+0x560   
                                             ( 0x3b, 0x9fffffff7f7e7c00, 0x0 )

// 

     r52: 0x0000000000000915  out2   
     r51: 0x0000000000000000  out1   
     r50: 0xe000000635acc280  out0   
     r49: 0x0000000000000002  loc14  
     r48: 0xe00000073511c504  loc13  
     r47: 0xe00000073511c502  loc12  
     r46: 0x0000000000000001  loc11  
     r45: 0xe0000006384ef200  loc10  
     r44: 0xe00000073511c538  loc9   
     r43: 0xe00000073511c508  loc8   
     r42: 0x0000000000000001  loc7   
     r41: 0x40000006384ef220  loc6   
     r40: 0xe00000073511c508  loc5   
     r39: 0x0000000000000000  loc4   
     r38: 0x0000000000000000  loc3   
     r37: 0xfffe7fffffff404f  loc2/pr
     r36: 0xe000000000e371d0  loc1/rp  reapfds+0x130
     r35: 0x0000001f3e3c8d9e  loc0/pfs sof=30 sol=27
     r34: 0xe000000937de9480  in2    
     r33: 0x0000000000000002  in1    
     r32: 0xe00000073511c500  in0    
0x9fffffff7f7e9458 0x9fffffff7f7e78d0 0xe000000000e27ad0 bbds_trim_size+0x2d0

// bbds_trim_size.s
                     ;;
+0x2c0               mov r51=0
                     nop.m 0x0 
                     br.call.dptk.many b0=kmem_arena_free
                     ;;
+0x2d0 L_0008:       ld8 r9=[r41]
                     ld4.a r8=[r40]
                     nop.i 0x0 
                     ;;
+0x2e0               ld8.a r10=[r41]
                     shladd r9=r42,3,r9
                     ;;

p4> p4_print i8 0x40000006384ef220
  0x40000006384ef220
  0x40000006384ef220 : 0x0000000000000000
  No translation

// r41
p4> grep r41 bbds_trim_size.s
                     adds r41=32,r45
+0x2d0 L_0008:       ld8 r9=[r41]
+0x2e0               ld8.a r10=[r41]
+0x300               ld8.c.clr r10=[r41]
+0x360 L_0011:       adds r41=6,r32
+0x370 L_0012:       ld2 r15=[r41]


     r45: 0xe0000006384ef200  loc10  
     r41: 0x40000006384ef220  loc6   

     adds r41=32,r45
     r41 = r45+32 = 0xe0000006384ef200 + 32 = 0xe0000006384ef220

p4> x "0xe0000006384ef200+32"
0xe0000006384ef220
p4> x "0xe0000006384ef220-0x40000006384ef220"
0xa000000000000000
p4> Let -b 0xa
1010

// 2 bits flipped. a bit unusual.

```

