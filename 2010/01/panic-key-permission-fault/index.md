# System panic with Key Permission Fault in KERNEL mode.

## Panic string as Unresolved kernel interruption.

```

System Crash Dump Analysis Report
=================================

Symptom
-------
 System panic with Key Permission Fault in KERNEL mode.
 Panic string as Unresolved kernel interruption.
 
Root Cause
----------
 This panics due to socket's stream closed unexpectedly.
 This problems is due to device major number conflict on
 the system. Major number conflict occurs when device files
 on the system happen to have the same major number with
 clonable device /dev/tcp or /dev/udp.

 Major number Lsdev>
    Character     Block       Driver          Class          MP_safe
       76          -1         tcp             pseudo           yes
       57          -1         mpt             ext_bus          yes

 There are two Ultra320 interface cards on system,
p4> grep mpt iot.txt
0/5/1/0    mpt    0xe00000013836cb00  1 CLAIMED   SCSI Ultra320 A6961-60011
0/5/1/1    mpt    0xe00000013836ce00  2 CLAIMED   SCSI Ultra320 A6961-60011

But what we have 4 special files under /dev
crw-rw-rw-   1 bin        bin         57 0x010000 Jul 17  2007 /dev/mpt1
crw-rw-rw-   1 bin        bin         57 0x020000 Jul 17  2007 /dev/mpt2
crw-rw-rw-   1 bin        bin         76 0x080000 Jul 17  2007 /dev/mpt8
crw-rw-rw-   1 bin        bin         76 0x090000 Jul 17  2007 /dev/mpt9

Action Plan
-----------
 Please remove the conflict device files, list as follows.
crw-rw-rw-   1 bin        bin         76 0x080000 Jul 17  2007 /dev/mpt8
crw-rw-rw-   1 bin        bin         76 0x090000 Jul 17  2007 /dev/mpt9


Detail Analysis
===============
                        =======================
                        = General Information =
                        =======================

Dump time Mon Jan 25 04:28:07 2010 UTC-8
System has been up 383 days, 13 hours, 44 minutes.

Node Name         : localhost
Model             : server rx6600
BIOS revision     : 02.03
HP-UX version     : B.11.23
Kernel whatstring : @(#) $Revision: vmunix:    B11.23_LR FLAVOR=perf Fri Aug 29
22:35:38 PDT 2003 $
Number of CPU's   : 4
Disabled CPU's    : 0
CPU type          : IA64 (1.59 Ghz)
CPU Architecture  : IA-64 0
Load average      : 0.15  0.19  0.25


                        ================
                        = Crash Events =
                        ================


Crash event 0 was a PANIC !
Panic string : post_hndlr(): Unresolved kernel interruption

Trap information
================

Getting trap information from save_state_t 0x28d58431.0x9fffffff7edc7800
Reason 0x51: Key Permission Fault

Interrupt Instruction Pointer:
        IIP = 0xdead31.0xe00000000171a700, slot 0

Interrupt Instruction Bundle:

spinlock+0x40():    (@ 0xe00000000171a700)
+0x40               fetchadd4.acq r23=[r27],1
                    ld4 r28=[r28]
                    cmp4.eq p6,p0=0,r25
                    ;;

Interrupt Faulting Address:
        IFA = 0x20958431.0x8

Note: The trapping address has a low offset, and is probably a
null pointer de-reference


Stack Trace for Crash event 0
=============================

==============  EVENT  ============================
= Event #0 is CT_PANIC on CPU #3;
= p crash_event_t 0xe000000100007000
= p rpb_t 0xe000000100d31340
= Process at 0xe00000018fd70000, pid 13403, "postmaster"
==============  EVENT  ============================
RR0=0x20958431  RR1=0x24958431  RR2=0x00004031  RR3=0x00004031
RR4=0x28d58431  RR5=0x00ffff31  RR6=0x00ffff31  RR7=0x00dead31
               BSP                 SP                 IP
0x9fffffff7edc9648 0x9fffffff7edc7780 0xe0000000016e97a0 panic+0x380
0x9fffffff7edc9548 0x9fffffff7edc77b0 0xe0000000005c0e00 post_hndlr+0xcc0
0x9fffffff7edc94b0 0x9fffffff7edc77d0 0xe0000000005bfc80 vm_hndlr+0x220
0x9fffffff7edc9488 0x9fffffff7edc77f0 0xe000000000d71780 bubbleup+0x880
+-------------  TRAP  ----------------------------
|  Key Permission Fault in KERNEL mode
|    IIP=0xe00000000171a700:0
|    IFA=0x8
|  p struct save_state 0x28d58431.0x9fffffff7edc7800
+-------------  TRAP  ----------------------------
0x9fffffff7edc9470 0x9fffffff7edc7ab0 0xe00000000171a700 spinlock+0x40
0x9fffffff7edc93a8 0x9fffffff7edc7ab0 0xe0000000006157e0 hpstreams_select_int2+0
x220
0x9fffffff7edc9388 0x9fffffff7edc7ab0 0xe0000000006167f0 streams_select2+0x30
0x9fffffff7edc9310 0x9fffffff7edc7ab0 0xe000000000616a50 soo_select+0x230
0x9fffffff7edc9230 0x9fffffff7edc7ae0 0xe0000000006dcbc0 selscan+0x270
0x9fffffff7edc9130 0x9fffffff7edc7b00 0xe0000000006dd970 select+0x4d0
0x9fffffff7edc9010 0x9fffffff7edc7bd0 0xe0000000006d7eb0 syscall+0x920
------------------------------------------------------
p4> crashinfo -ofiles 13403 -v
Command line: crashinfo -ofiles 13403 -v

pid   p_maxof ofiles p_comm
----- ------- ------ ------
13403 2048    15     postmaster
   cwd  vnode=0xe000000189e560c0: VVXFS/VDIR v_data=0xe00000018fac8000 /var/opt/wbem
   fd=0 file=0xe00000018acea500 vnode=0xe0000001829531c8: VNFS_SPEC/VCHR v_data=0xe0000001829531c0 v_rdev=0x03000002
   fd=1 file=0xe00000018884e900 vnode=0xe000000189828440: VVXFS/VREG v_data=0xe00000018e830ab0 /var/opt/sfmdb/pgsql/sfmdb.log
   fd=2 file=0xe00000018884e900 vnode=0xe000000189828440: VVXFS/VREG v_data=0xe00000018e830ab0 /var/opt/sfmdb/pgsql/sfmdb.log
   fd=3 file=0xe00000018aa01d80 vnode=0xe00000018b56c3c8: VNFS_SPEC/VCHR v_data=0xe00000018b56c3c0 v_rdev=0x15000000
   fd=4 file=0xe00000018aceab80 socket=0xe000000189e00740: AF_INET/STREAM sth=0xe000000189ce1c00 tcp_t=0xe00000018a3dab28 L=127.0.0.1:50547 (LISTEN) ip ipc_t=0xe000000189ce03e8
   fd=5 file=0xe00000018884e100 vnode=0xe00000018a157e48: PIPES/VFIFO v_data=0xe00000018a157e40
   fd=6 file=0xe00000018ac443c0 socket=0xe00000018ada8580: AF_UNIX/DGRAM R/S-Q=0/0 Inode=0xe00000018843c040 Addr=/var/spool/sockets/pwgr/client13403
   fd=7 file=0xe00000018a7d7240 socket=0xe00000018ad25200: AF_INET6/STREAM sth=0xe00000018adfd7c0 tcp_t=0xe00000018ab54b28 L=::,10864 (LISTEN) ip6 ipc_t=0xe00000018ada9ce8
   fd=8 file=0xe00000018ad1fc80 socket=0xe00000018ac67740: AF_INET6/STREAM sth=0xe0000001882f4400 tcp_t=0xe0000001882fa5e8 L=127.0.0.1:50548 R=127.0.0.1:1712 (ESTABLISHED) ip6 ipc_t=0xe0000001888350e8
   fd=9 file=0xe00000018a7d70c0 socket=0xe00000018ada8ac0: AF_INET/STREAM sth=0xe00000018adb9880
   fd=10 file=0xe000000183290340 vnode=0xe0000001832d2588: PIPES/VFIFO v_data=0xe0000001832d2580
   fd=11 file=0xe00000018ac44040 socket=0xe00000018ac673c0: AF_UNIX/STREAM q0len=0 q0=0x0 qlen=0 q=0x0 (LISTEN) Inode=0xe0000001883e5b00 Addr=/tmp/.s.PGSQL.10864
   fd=12 file=0xe00000018aa01e80 socket=0xe00000018ac67200: AF_INET/DGRAM sth=0xe00000018adfd040 udp_t=0xe00000018ac47768 L=127.0.0.1:50302  T=127.0.0.1:50302 (CONNECTED) ip ipc_t=0xe00000018ada9868
   fd=13 file=0xe00000018ac444c0 vnode=0xe00000018ac70e48: PIPES/VFIFO v_data=0xe00000018ac70e40
   fd=14 file=0xe00000018a7d73c0 vnode=0xe00000018ac70e48: PIPES/VFIFO v_data=0xe00000018ac70e40

Executing user command: /opt/ktools/bin/strshow-a-i -p 0xe00000018adb9880

============= Drawing stream at 0xe00000018adb9880 =============

 write side  read side                 WQ@           RQ@

         sth           level 0     (0xe00000018addcd80,  0xe00000018addcc00)


============================ basics ============================

 sth_flags       0x0008c800    F_STH_KERN_IF | F_STH_CLOSED | F_STH_CLOSING | F_STH_WAKE_ALL
 sth_dev         0x4c080000    device number ==> major 76, minor 0x80000

p4> kmeminfo -v 0xe00000018addcc00
tool: kmeminfo 9.16 - libp4 9.373 - libhpux 1.272
unix: ./vmunix 11.23 64bit IA64 on host "localhost"
core: ./INDEX
link: Tue Jan 06 14:35:02 EAT 2009
boot: Tue Jan  6 14:43:12 2009 UTC-8
time: Mon Jan 25 04:28:07 2010 UTC-8
nbpg: 4096 bytes


VA 0xdead31.0xe00000018addcc00 translates to PA 0x22ddcc00
  Page table entry: pte_t 0xe0000001601b10c0
    Access rights : 0x0c PTE_AR_KRWX
    Protection key: 0xbeef KERNEL/PUBLIC
    Page size     : 64MB
    Large page details:
      Addr :            virtual    physical
      Start: 0xe000000188000000  0x20000000
      End  : 0xe00000018c000000  0x24000000
PA 0x22ddcc00 is classified as "PC_KDDATA" (dumped):
  Page classification: pgclass_t 0xe000000101fc189c
Locality for PA 0x22ddcc00 is loc_t 0, loc index 0, loc type CLM.

VA belongs to variable arena "M_STRQUEUE":
  cpu 3, index 11, size 952 bytes
VM KMEM related structures:
  kmem_arena_t     0xe00000013800ba40
  kmem_flist_hdr_t 0xe00000013c17e980
  kmem_obj_hdr_t   0xe00000018addcbf8
VA is within the object at:
      Start: 0xe00000018addcc00
      Size : 0x00000000000003b8  952 bytes
      End  : 0xe00000018addcfb8
The object is free.

Kmem arena : kmem_arena_t      0xe00000013800ba40
Free list  : kmem_flist_hdr_t  0xe00000013c17e980
Page layout:
 pg hdr    : kmem_sobj_pghdr_t 0xe00000018addc000
 pg hdr end:                   0xe00000018addc028
 objs      :                   0xe00000018addc0b8
 obj hdr   : kmem_obj_hdr_t    0xe00000018addcbf8  ko_fnext=0xe00000018ade00f8
 obj       :                   0xe00000018addcc00
 >>> VA >>>:                   0xe00000018addcc00  within object (offset 0).
 obj end   :                   0xe00000018addcfb8
 objs end  :                   0xe00000018addcfb8

```

