# System panics with data page fault in scsi_dequeue.

## System panics with data page fault in scsi_dequeue.

```


System crash dump analysis report
=================================
Symptom
-------
  System panics with data page fault in scsi_dequeue.

Root Cause
----------
  DPF (Data Page Fault) panic was a bad av_forw pointer
  in the buffer header which was not properly unlinked.

Action Plan
-----------
  Please install I/O forwarding patch PHKL_20473  

Detail Analysis
---------------

			=======================
			= General Information =
			=======================

Dump time Wed Jul 19 14:30:57 2006 UTC-8
System has been up 16 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/L2000-44
HP-UX version    : B.11.0 (64-bit Kernel)
Number of CPU's  : 2
Disabled CPU's   : 0
CPU type         : PCXW (440 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 0.66  0.72  0.52  

			================
			= Crash Events =
			================

Note: Crash event 0 was a PANIC !

Panic string : Data page fault

Getting trap information from trap marker at 0x0.0x804920

Trap information
================

Type 15: Data TLB Miss Fault/Data Page Fault

Interruption Instruction Register:
	IIR = 0x73fd0040

Interruption Space and Offset Registers:
	ISR.IOR = 0x0.0x20

Interruption Instruction Address Queue:
	PCSQ.PCOQ = 0x0.0x183718 = scsi_dequeue+0x30

Interrupt Instruction at scsi_dequeue+0x30:
	std  ret1,0x20(r31)

Stack Trace for Crash event 0
=============================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x872608
= Using pc from pim.wide.rp_rp_hi = 0x35d1b4
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x0000000000804f80 0x0035d1b4 panic+0x14
0x0000000000804f00 0x0037121c report_trap_or_int_and_panic+0x84
0x0000000000804e80 0x0037075c interrupt+0x1d4
0x0000000000804df0 0x00137588 ihandler+0x928
+-------------  TRAP  ----------------------------
|  Trap type 15 in KERNEL mode at 0x183718 (scsi_dequeue+0x30)
|  p struct save_state 0.0x804920
+-------------  TRAP  ----------------------------
SR4=0x00000000
                SP         RP Return Name
0x0000000000804920 0x00183718 scsi_dequeue+0x30   
0x0000000000804920 0x005cafcc fcparray_start+0x74
0x0000000000804860 0x0016a5d8 scsi_strategy_real+0x370
0x0000000000804690 0x0044b8cc ioforw_int+0xcc
0x0000000000804600 0x001369b0 mp_ext_interrupt+0x160
0x0000000000804520 0x00137564 ihandler+0x904
+-------------  TRAP  ----------------------------
|  Trap type 4 in KERNEL mode at 0x1070f8 (disksort_dequeue+0x40)
|  p struct save_state 0.0x804050
+-------------  TRAP  ----------------------------
SR5=0x01a10c00
                SP         RP Return Name
0x400003ffffff1bc0 0x001070f8 disksort_dequeue+0x40
0x400003ffffff1ac0 0x001074ac lv_strategy+0x14c
0x400003ffffff19e0 0x001a88e8 vx_dev_strategy+0x98
0x400003ffffff1930 0x0018dda4 vx_flush_chain+0x3cc
0x400003ffffff1840 0x0018e384 vx_vnode_flush+0x58c
0x400003ffffff1640 0x0010d9b0 vx_do_putpage+0xc0
0x400003ffffff1570 0x001ba020 vx_write_flush+0x90
0x400003ffffff14b0 0x00163cd4 vx_write_default+0x28c
0x400003ffffff12b0 0x00165e00 vx_write1+0x4d8
0x400003ffffff1130 0x00166ddc vx_rdwr+0x164
0x400003ffffff1040 0x003dbfc4 vno_rw+0x84
0x400003ffffff0f70 0x00161eac write+0x104
0x400003ffffff0e60 0x00162cd4 syscall+0x6fc
0x400003ffffff0c70 0x00035944 syscallinit+0x54c

Stack Trace for Crash event 0 with all args
===========================================

==============  EVENT  ============================
= Event #0 is PANIC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x872608
= Using pc from pim.wide.rp_rp_hi = 0x35d1b4
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x0000000000804f80 0x0035d1b4 panic+0x14
0x0000000000804f00 0x0037121c report_trap_or_int_and_panic+0x84
         arg0: 0x0000000000000002
         arg1: 0x000000000000000f
         arg2: 0x0000000000804920
0x0000000000804e80 0x0037075c interrupt+0x1d4
         ....  --------n/a-------
         arg1: 0x0000000000804920
0x0000000000804df0 0x00137588 ihandler+0x928
+-------------  TRAP  ----------------------------
|  Trap type 15 in KERNEL mode at 0x183718 (scsi_dequeue+0x30)
|  p struct save_state 0.0x804920
+-------------  TRAP  ----------------------------
SR4=0x00000000
                SP         RP Return Name
0x0000000000804920 0x00183718 scsi_dequeue+0x30
         arg0: 0x000000004076da18    // ----------------
         arg1: 0x0000000000000001
0x0000000000804920 0x005cafcc fcparray_start+0x74
         arg0: 0x000000004076c000
         ....  --------n/a-------
         arg5: 0x000000004016f980
0x0000000000804860 0x0016a5d8 scsi_strategy_real+0x370
         arg0: 0x0000000048a18000
0x0000000000804690 0x0044b8cc ioforw_int+0xcc
0x0000000000804600 0x001369b0 mp_ext_interrupt+0x160
         arg0: 0x0000000000804050
0x0000000000804520 0x00137564 ihandler+0x904
+-------------  TRAP  ----------------------------
|  Trap type 4 in KERNEL mode at 0x1070f8 (disksort_dequeue+0x40)
|  p struct save_state 0.0x804050
+-------------  TRAP  ----------------------------
SR5=0x01a10c00
                SP         RP Return Name
0x400003ffffff1bc0 0x001070f8 disksort_dequeue+0x40
         arg0: 0x00000000488d3308      
0x400003ffffff1ac0 0x001074ac lv_strategy+0x14c
0x400003ffffff19e0 0x001a88e8 vx_dev_strategy+0x98
         arg0: 0x0000000048a85a08
0x400003ffffff1930 0x0018dda4 vx_flush_chain+0x3cc
         arg0: 0x000000004a8aa000
         arg1: 0x400003ffffff1738
         arg2: 0x0000000000000100
         arg3: 0x400003ffffff1758
0x400003ffffff1840 0x0018e384 vx_vnode_flush+0x58c
         arg0: 0x000000004a8aa000
         arg1: 0x0000000000000000
         arg2: 0x0000000000000100
         arg3: 0x0000000000022ec0
         arg4: 0x0000000000022ec8
0x400003ffffff1640 0x0010d9b0 vx_do_putpage+0xc0
         arg0: 0x000000004a8aa000
         ....  --------n/a-------
         arg3: 0x0000000000000100
0x400003ffffff1570 0x001ba020 vx_write_flush+0x90
         arg0: 0x000000004a8aa0b0
         arg1: 0x0000000000000100
         arg2: 0x0000000000000002
0x400003ffffff14b0 0x00163cd4 vx_write_default+0x28c
         arg0: 0x000000004a8aa0b0
         arg1: 0x400003ffffff0ea8
         arg2: 0x0000000045d90000
         arg3: 0x0000000000000002
         arg4: 0x0000000000000000
         arg5: 0x0000000000000001
0x400003ffffff12b0 0x00165e00 vx_write1+0x4d8
         arg0: 0x000000004a8aa000
         arg1: 0x400003ffffff0ea8
         arg2: 0x0000000000000001
0x400003ffffff1130 0x00166ddc vx_rdwr+0x164
         ....  --------n/a-------
         arg1: 0x400003ffffff0ea8
         arg2: 0x0000000000000002
         arg3: 0x0000000000000001
0x400003ffffff1040 0x003dbfc4 vno_rw+0x84
         arg0: 0x0000000003d4a870
         arg1: 0x0000000000000002
         arg2: 0x400003ffffff0ea8
         arg3: 0x00000000003dbf40
0x400003ffffff0f70 0x00161eac write+0x104
         arg0: 0x400003ffffff03a0
         ....  --------n/a-------
         arg6: 0x0000000000373618
0x400003ffffff0e60 0x00162cd4 syscall+0x6fc
0x400003ffffff0c70 0x00035944 syscallinit+0x54c

q4> load struct buf from 0x0`4076da18 next av_forw max 10
loaded 2 struct bufs as a linked list 

(stopped by bad read at 0x0.0x18191a1b'1c1d1e1f)

q4> print -x addrof av_forw av_back b_dev b_flags
   addrof    av_forw    av_back b_dev b_flags
0x4076da18 0x4076ee00 0x4076eefd     0       0
0x4076ee00 0x18191a1b1c1d1e1f 0x2021222324252627 0x4c4d4e4f 0x10203

#  what vmunix | grep io_forw
        io_forw.c $Date: 1999/05/10 00:33:25 $Revision: 
        r11ros/cup_ros_ep3_pb/2 PATCH_11.00 (PHKL_18543)

```

