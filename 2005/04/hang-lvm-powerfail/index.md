# System hang in the LVM POWERFAIL queue.

## System hang in the LVM POWERFAIL queue.
  
```

System crash dump analysis report
=================================
Symptom
-------
  System behaved as partial hang, no response on telnet/login.
  TOC dump be taken for root cause finding.

Cause
-----
  Partial AC power source of disk rack lost, which leads the fc60
  disk array de-function and LVM sub-system shows powerfail event.

  Ends up all buffer header be exhausted utilized and system appears
  as hang.

Action Plan
-----------
  1. Redundant check the fiber loops on disk rack. 
  2. Install the LVM patch and dependencies as follows:
     (10 patches 47MB)

     PHKL_32920  LVM Cumulative Patch
     PHCO_21187  cumulative SAM/ObAM patch
     PHCO_23651  fsck_vxfs(1M) cumulative patch
     PHCO_23791  LVM commands cumulative patch
     PHKL_18543  PM/VM/UFS/async/scsi/io/DMAPI/JFS/perf patch
     PHKL_19202  fsadm panic if extending root on 11.x
     PHKL_20016  2nd CPU not recognized in G70/H70/I70
     PHKL_22589  LOFS, select(), IDS/9000 and umount race fix
     PHKL_27980  VxFS 3.1 cumulative patch: CR_EIEM
     PHKL_28766  Probe,IDDS,PM,VM,PA-8700,AIO,T600,FS,PDC,CLK

Detail Analysis
---------------

The dump showed the problem to be an IO induced hang.
What could be seen was that a lot of IOs, almost 17629 transactions,
were waiting within the LVM subsystem on what is called
the POWERFAIL queue.

Powerfail:
    Flags:                0x1
    powerfail wait queue:
    pf_wait_Q      @ 0x008e7be8
    0x008e7be8 : 
    0x008e7be8 struct lv_queue {
    0x008e7be8   struct buf *lv_head;                  0x00000000_4f8e0800
    0x008e7bf0   struct buf **lv_tail;                 0x00000000_43d69018
    0x008e7bf8   int     lv_count;                     17629
    0x008e7c00 };
    
    Addr     av_forw   b_tid    b_flags b_error    b_blkno b_qstart.tv_sec 
     0x4f8e0800  0x539a1400   13236   0x240188       0   0x1a5e40 0x0184919c 
     0x539a1400  0x6dfa7000   13236   0x240188       0   0x1a5e60 0x0184919c 
     0x6dfa7000  0x6ec67400   13236   0x240188       0   0x1a6d40 0x0184919c 
     0x6ec67400  0x654a6000   13236   0x240188       0   0x1a6d60 0x0184919c 
     0x654a6000  0x63866400   13236   0x240188       0   0x1a6cc0 0x0184919c 
.
.

These IOs were waiting for LVM to switch from the primary
link to the alternate link. But the alternate link fail.

SCSI: Async write error -- dev: b 31 0x0d0000, errno: 126, resid: 32768,
 blkno: 18217568, sectno: 36435136, offset: 1474920448, bcount: 32768.

LVM: Path (device 0x1f0c0000) to PV 0 in VG 2 Failed!
LVM: vg[2]: pvnum=0 (dev_t=0x1f0d0000) is POWERFAILED
LVM: vg[2]: pvnum=0 (dev_t=0x1f0d0000) is POWERFAILED
lv_readvgdats: Could not read VGDA 1 header & trailer from
    disk H/W path 0/4/0/0.8.0.3.0.0.0 (error = 5)
lv_readvgdats: Could not read VGDA 1 header & trailer from
    disk H/W path 0/4/0/0.8.0.3.0.0.0 (error = 5)
lv_readvgdats: Could not read VGDA 1 header & trailer from
    disk H/W path 0/4/0/0.8.0.3.0.0.0 (error = 5)
LVM: Failed to restore PV 0 to VG 2!

The buffer headers were at the time of the dump completely out.
As the most common wait channel shows 36 process are wait in
bfreelist as follows. Look at the getty process on console, which
is included in here also wait on bfreelist, this is the reason way
there is no response on console login. 
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
bfreelist                                          36     1004679    560040

TID     PID   PPID  TICKS      PRI SPU STAT  SYSCALL  COMMAND        WCHAN
-----   ----- ----- ------     --- --- ----- -------  -------        ---------
1293    1236  1     560040     149 0   SLEEP read     getty          bfreelist
13240   358   21529 587143     149 2   SLEEP stat     diagmond       bfreelist
22009   22474 1     672776     149 0   SLEEP access   dm_TL_adapter  bfreelist
724     667   1     678081     149 3   SLEEP stat     inetd          bfreelist
21983   22448 1     764846     149 3   SLEEP access   dm_FCMS_adapte bfreelist
21934   22399 1     791375     149 0   SLEEP access   disk_em        bfreelist
22283   22742 1     828346     149 2   SLEEP access   lpmc_em        bfreelist
1314    1257  1234  837220     149 3   SLEEP read     dtlogin        bfreelist
1123    1066  1     858517     149 0   SLEEP access   cron           bfreelist
21753   22083 21529 897042     149 0   SLEEP access   memlogd        bfreelist
22340   22799 1     940481     149 2   SLEEP access   sysstat_em     bfreelist
13239   357   1019  944980     149 1   SLEEP execve   dced           bfreelist
22051   22516 1     955739     149 0   SLEEP access   dm_core_hw     bfreelist
3300    28786 1     972359     149 2   SLEEP read     mad            bfreelist
22225   22684 1     976850     149 3   SLEEP access   fc60mon        bfreelist
1086    1029  1     995800     64  0   SLEEP creat    rbootd         bfreelist
16700   17073 1     1000794    149 3   SLEEP readv    oracle         bfreelist
16807   17156 1     1003872    149 1   SLEEP write    tnslsnr        bfreelist
22348   22677 22147 1004679    149 0   SLEEP stat     p_client       bfreelist
536     479   1     1004679    149 3   SLEEP writev   syslogd        bfreelist
48      35    0     1004679    149 0   SLEEP n/a      vxfsd          bfreelist
23993   24437 1     1004679    149 0   SLEEP stat     p_client       bfreelist
21672   22137 1     1004679    149 1   SLEEP stat     p_client       bfreelist
21134   21465 1     1004679    149 1   SLEEP stat     p_client       bfreelist
13236   355   349   1004679    149 1   SLEEP write    rcp            bfreelist
21209   21540 25073 1004679    149 1   SLEEP stat     p_client       bfreelist
1145    1088  1     1004679    149 0   SLEEP mknod    AM60Srvr       bfreelist
22177   22636 1     1004679    149 3   SLEEP access   dm_stape       bfreelist
15166   15519 15516 1004679    149 3   SLEEP read     dbsnmp         bfreelist
21752   22082 21529 1004679    149 3   SLEEP read     diaglogd       bfreelist
50      35    0     1004679    149 2   SLEEP n/a      vxfsd          bfreelist
13105   226   225   1004679    90  1   SLEEP write    sadc           bfreelist
7300    28550 1     1004679    149 3   SLEEP stat64   perl           bfreelist
22105   22434 22090 1004679    149 3   SLEEP stat     p_client       bfreelist
52      35    0     1004679    149 0   SLEEP n/a      vxfsd          bfreelist
1108    1051  1     1004679    149 0   SLEEP stat     pwgrd          bfreelist

The majority of these IOs were for the following prealloc process.

count   tid     syscall process
------- ------- ------- -------
17487	13236	write	rcp
76	1145	mknod	AM60Srvr
11	40	n/a	vxfsd
9	21134	stat	p_client
9	15167	ksleep	dbsnmp
5	23993	stat	p_client
5	21751	access	cclogd
5	13237	read	diaglogd
4	21752	stat	p_client
3	22348	stat64	perl
2	7300	stat	p_client
2	22105	stat	p_client
2	21672	stat	p_client
2	21209	write	oracle
2	13061	writev	syslogd
1	536	n/a	vxfsd
1	36	access	dm_stape
1	22177	write	oracle
1	16698	write	oracle
1	16696	write	oracle		


Looking at thread detail who is the most utilizer of buffer header 
That is the rcp process.

proc[181] pid=355 tid=13236  cmd="/usr/bin/rcp -p dw1:/work/dmp/*x* /uploa"
Process  : p proc_t    0x3cb4740
           proc[181] pid=355 rcp
Kthread  : p kthread_t 0x41380f0
Using PCB: p user_t 0xb62d400.0x400003ffffff0000
FUNC
_swtch+0xd4
_sleep+0x154
getnewbuf_desperate+0x370
getnewbuf+0x660
allocbuf1+0x178
brealloc1+0x64
ogetblk+0x254
getblk1+0x260
vx_getblk+0x50
vx_write_default+0x564
vx_write1+0xd90
vx_rdwr+0x164
vno_rw+0x84
write+0x104
syscall+0x28c
syscallinit+0x54c

The rcp thread was reading from dw1:/work/dmp/*x*  and
writing /upload_L. Look at the mount table below.

/upload_L file system came from /dev/vg02/lvol4 which is on the failed VG.
This indicates the reason why the buffer headers can not be released
due to the I/O transactions were not be completed, and ends up
all buffer header be exhausted utilized and system appears as hang.

Dev_t      Devicefile                 Mount point              Type struct vfs
0x40000003 /dev/root                  /                        vxfs 0x427b2000
0x40000001 /dev/vg00/lvol1            /stand                    hfs 0x42d50000
0x40020005 /dev/vg02/lvol5            /work_L                  vxfs 0x42d5a000
0x40000008 /dev/vg00/lvol8            /var                     vxfs 0x42ead000
0x40000007 /dev/vg00/lvol7            /usr                     vxfs 0x42ec9000
0x40020004 /dev/vg02/lvol4            /upload_L                vxfs 0x42ee1800
0x40000004 /dev/vg00/lvol4            /tmp                     vxfs 0x43198800
0x40000009 /dev/vg00/lvol9            /temp                    vxfs 0x431a3800
0x40020007 /dev/vg02/lvol7            /ora06                   vxfs 0x431bb800
0x40020006 /dev/vg02/lvol6            /ora05                   vxfs 0x431e3800
0x40020003 /dev/vg02/lvol3            /ora04                   vxfs 0x42f22800
0x40020002 /dev/vg02/lvol2            /ora03                   vxfs 0x42f47800
0x40020001 /dev/vg02/lvol1            /ora02                   vxfs 0x42f65000
0x40010001 /dev/vg01/lvol1            /ora01                   vxfs 0x42fb9000
0x40000006 /dev/vg00/lvol6            /opt                     vxfs 0x42fe4000
0x40000005 /dev/vg00/lvol5            /home                    vxfs 0x43201000
0x05000000 localhost:(pid639)               /net                      nfs 0x4c6e5000
0x05000001 localhost:(pid639)               /nfs_dw2                  nfs 0x4c6e5800

For more info grep from crash dump be shown here for reference.

			=======================
			= General Information =
			=======================

Dump time Thu Apr 28 11:22:05 2005 UTC-8
System has been up 294 days, 20 hours, 30 minutes.

System Name      : HP-UX
Node Name        : localhost
Model            : 9000/800/L2000-44
HP-UX version    : B.11.00 (64-bit Kernel)
Number of CPU's  : 4
Disabled CPU's   : 0
CPU type         : PCXW (440 Mhz)
CPU Architecture : PA-RISC 2.0
Load average     : 9.25  9.25  9.25  

			================
			= Crash Events =
			================

Note: Crash event 0 was a TOC !

Stack Trace for crash event 0
=============================

==============  EVENT  ============================
= Event #0 is TOC on CPU #0
= p crash_event_t 0x22000
= p rpb_t 0x7ebb58
= Using pc from pim.wide.rp_pcoq_head_hi = 0x12c938
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x00000000088d22b0 0x0012c938 idle+0xee8
0x00000000088d2050 0x0012f1f4 swidle+0x20


Stack Traces for other processors 
=================================


Processor #1
==============  EVENT  ============================
= Event #1 is TOC on CPU #1
= p crash_event_t 0x22030
= p rpb_t 0xd42370
= Using pc from pim.wide.rp_pcoq_head_hi = 0x12c874
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x00000000088d52b0 0x0012c874 idle+0xe24
0x00000000088d5050 0x0012f1f4 swidle+0x20

Processor #2
==============  EVENT  ============================
= Event #2 is TOC on CPU #2
= p crash_event_t 0x22060
= p rpb_t 0xd42a50
= Using pc from pim.wide.rp_pcoq_head_hi = 0x12c874
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x00000000088d82b0 0x0012c874 idle+0xe24
0x00000000088d8050 0x0012f1f4 swidle+0x20

Processor #3
==============  EVENT  ============================
= Event #3 is TOC on CPU #3
= p crash_event_t 0x22090
= p rpb_t 0xd43130
= Using pc from pim.wide.rp_pcoq_head_hi = 0x12cb70
==============  EVENT  ============================
SR4=0x00000000
                SP         RP Return Name
0x00000000088db2b0 0x0012cb70 idle+0x1120
0x00000000088db050 0x0012f1f4 swidle+0x20

			==================
			= Message Buffer =
			==================
.
.
.

SCSI: Async write error -- dev: b 31 0x0d0000, errno: 126, resid: 32768,
 blkno: 18217568, sectno: 36435136, offset: 1474920448, bcount: 32768.

LVM: Path (device 0x1f0c0000) to PV 0 in VG 2 Failed!
LVM: vg[2]: pvnum=0 (dev_t=0x1f0d0000) is POWERFAILED
LVM: vg[2]: pvnum=0 (dev_t=0x1f0d0000) is POWERFAILED
lv_readvgdats: Could not read VGDA 1 header & trailer from 
    disk H/W path 0/4/0/0.8.0.3.0.0.0 (error = 5)
lv_readvgdats: Could not read VGDA 1 header & trailer from 
    disk H/W path 0/4/0/0.8.0.3.0.0.0 (error = 5)
lv_readvgdats: Could not read VGDA 1 header & trailer from 
    disk H/W path 0/4/0/0.8.0.3.0.0.0 (error = 5)
LVM: Failed to restore PV 0 to VG 2!

DIAGNOSTIC SYSTEM WARNING:
   The diagnostic logging facility has started receiving excessive
   errors from the I/O subsystem.  I/O error entries will be lost
   until the cause of the excessive I/O logging is corrected.
   If the diaglogd daemon is not active, use the Daemon Startup command
   in stm to start it.
   If the diaglogd daemon is active, use the logtool utility in stm
   to determine which I/O subsystem is logging excessive errors.

lkno: 18218176, sectno: 36436352, offset: 1475543040, bcount: 32768.

SCSI: Async write error -- dev: b 31 0x0d0000, errno: 126, resid: 32768,
 blkno: 18218144, sectno: 36436288, offset: 1475510272, bcount: 32768.
.
.
.
		=======================================
		= Crash Event / Processor Information =
		=======================================

Number of processors = 4


        s
        t
        a       spin reg eiem/spl         eirr             ipsw            
evt cpu t type  dpth src cr15             cr23             cr22            
--- --- - ----- ---- --- ---------------- ---------------- ----------------
0   0   E TOC   0    rpb fffffff0ffffffff 0000000000000000 0804fd1f WCRQPDI
1   1   E TOC   0    rpb fffffff0ffffffff 0000000100000000 0804fc1f WCRQPDI
2   2   E TOC   0    rpb fffffff0ffffffff 0000000100000000 0804fc1f WCRQPDI
3   3   E TOC   0    rpb fffffff0ffffffff 0000000100000000 080cfe1f WBCRQPDI

Outstanding external interrupts
===============================

    eirr
cpu bit  SPL                Handler SPL        Handler
--- ---- --------           -----------        -------
1   31   SPLNOPREEMPT       SPLNOPREEMPT       take_a_trap
2   31   SPLNOPREEMPT       SPLNOPREEMPT       take_a_trap
3   31   SPLNOPREEMPT       SPLNOPREEMPT       take_a_trap

SPL/EIEM values:

0xfffffffeffffffff = SPLPREEMPTOK - Default user mode SPL level.
0xfffffff0ffffffff = SPLNOPREEMPT - Disable kernel preemption 
                                    (scheduling interrupt off).
0xffffff00ffffffff = SPL2 - Disable software interrupt (software triggers off).
0xef00080000000000 = SPL5 - Disable IO modules.
0xc700000000000000 = SPL6+CLOCK_RESYNC - Disable hardclock+enable clock-resync.
0xc600000000000000 = SPL6 - Disable hardclock.
0x0000000700000000 = SPL7/PSW_I=0 - Disable the world.

			========================
			= Processor Clock Info =
			========================

hardclock_late = 0
itick_per_tick = 4400000
lbolt          = 2547553244 (0x97d893dc)

    mpi                interval                         clk eiem eirr PSW
cpu timeinval          timer              delta (ticks) od  0,4  0,4  I
--- ------------------ ------------------ ------------- --- ---- ---- ---
0   0x27d2d5e35dbf2a   0x27d2d5e321f3b3   0             0   1 1  0 0  1 
1   0x27d2d5f7950adf   0x27d2d5f7898c9a   0             0   1 1  0 0  1 
2   0x27d2d5f7946ef4   0x27d2d5f7899d05   0             0   1 1  0 0  1 
3   0x27d2d5f7953ae0   0x27d2d5f789a25b   0             0   1 1  0 0  1 

			=================
			= Syswait Array =
			=================

cpu iowait
--- ------
0   1
1   1
2   2

			=================
			= Load Averages =
			=================
avenrun
=======
9.25  9.25  9.25  

real_run
========
0.000002  0.003519  0.006062  

pwrun ("fast" io wait)
======================
37.000000  37.000000  36.994405  

mp_avenrun
==========
cpu0 : 13.000000  13.000030  12.997752  
cpu1 : 7.000002  7.003489  7.005135  
cpu2 : 6.000000  6.000000  5.998375  
cpu3 : 11.000000  11.000000  10.999205  

			======================
			= Thread Information =
			======================

18 Threads ran in the last second
36 Threads ran in the last 5 seconds
37 Threads ran in the last 10 seconds
62 Threads ran in the last minute
125 Threads ran in the last hour

statdaemon ran 90 ticks ago


Most Common Wait Channels
=========================
                                                          ticks since run:
Wait Channel                                       count  longest    shortest
------------                                       -----  ---------- ----------
bfreelist                                          36     1004679    560040    
selwait                                            23     121398017  0         
vx_inactive_thread_sv+0x18                         19     36450      450       
vx_inactive_thread_sv+0x10                         14     26450      450       
vx_inactive_thread_sv                              8      14450      450       
vx_inactive_thread_sv+0x8                          6      10450      450       
lvmkd_q                                            6      226        224       
async_bufhead                                      4      2547531621 2547531621
streams_mp_sync                                    4      1460654    1460650   
LVM vg02/lv6                                       3      1015664    1015518   
streams_blk_sync                                   2      2547542680 2547542680
vx_sleep_lock(0x51ecc380)                          2      1011346    1011327   


Most Common Sleep Callers
=========================
                                                          ticks since run:
Sleep Caller                                       count  longest    shortest
------------                                       -----  ---------- ----------
vx_inactive_thread()                               47     36450      450       
hpstreams_read_int()                               37     1015518    42        
getnewbuf_desperate()                              36     1004679    560040    
wait1()                                            27     2547528584 1032398   
select()                                           15     121398017  0         
ksleep()                                           15     1035890918 2732      
poll()                                             8      121398017  72        
semop()                                            8      739255771  42        
lvmkd_daemon()                                     6      226        224       
svc_run()                                          4      1823815116 5603615   
async_daemon()                                     4      2547531621 2547531621
biowait()                                          4      1015664    1015164   
pm_sigwait()                                       3      99         45        
fifo_rdwr()                                        3      2547529752 980865    
streams_getmsg()                                   2      190622811  190621512 
vx_sleep_lock()                                    2      1011346    1011327   

```

