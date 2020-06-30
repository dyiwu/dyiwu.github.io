# MC/SG TOC system due to MC/ServiceGuard daemon cmcld aborted.

## MC/SG TOC system due to MC/ServiceGuard daemon cmcld aborted.

```

System crash dump analysis report
=================================
Symptom
-------
  MC/ServiceGuard unable to maintain contact with cmcld daemon.
  System was brought down through a TOC by the safety timer mechanism
  to ensure data integrity.

Root Cause
----------
  'cmcld' - received SIGABRT

Action Plan
-----------
 Please apply the patches for MC/SG and dependencies.

 PHSS_30742  MC/ServiceGuard and SG-OPS Edition A.11.13
 PHSS_30743  Cluster Object Manager A.01.03.01

 PHCO_23651  fsck_vxfs(1M) cumulative patch
 PHCO_29380  user/group(add/mod/del)(1M) cumulative patch
 PHCO_31879  cumulative SAM/ObAM patch
 PHCO_33244  LVM commands cumulative patch
 PHKL_18543  PM/VM/UFS/async/scsi/io/DMAPI/JFS/perf patch
 PHKL_19202  fsadm panic if extending root on 11.x
 PHKL_20016  2nd CPU not recognized in G70/H70/I70
 PHKL_23409  NFS, Large Data Space, kernel memory leak
 PHKL_30972  hang in aio_shutdown_fd_begin();EVP
 PHKL_32986  VxFS sendfile reallocation panic
 PHKL_33268  callout/corruption/abstime/sleep/mpctl patch
 PHKL_34023  LVM Cumulative Patch
 PHKL_34341  Probe,IDDS,PM,VM,PA-8700,AIO,T600,FS,PDC,CLK
 PHNE_28525  Cumulative STREAMS Patch
 PHNE_29773  sendmail(1m) 8.9.3 patch
 PHNE_33395  cumulative ARPA Transport patch


Detail Analysis
---------------

Mar 13 13:44:09 localhost cmcld: 
    Interrupted system call.  This may be caused by a very high system load.
    Unable to send DLPI message, Invalid argument
    cl_abort: abort cl_kepd_printf failed: Invalid argument
    Aborting! Failed to send over DLPI

Mar 13 13:44:18 localhost cmlvmd: 
  Could not read messages from /usr/lbin/cmcld:
  Software caused connection abort

Mar 13 13:44:18 localhost cmsrvassistd[5282]: 
    Lost connection with ServiceGuard cluster daemon (cmcld): 
    Software caused connection abort

Mar 13 13:44:18 localhost cmclconfd[5277]: 
    The ServiceGuard daemon, /usr/lbin/cmcld[5278],
    died upon receiving the signal 6.

localhost-root:/var/tombstones# ll /var/adm/cmcluster total 32326
-rw-r--r--   1 root       sys              8 Mar 13 15:28 .cm_start_time
----------   1 root       root             0 Sep  8  2000 config.lck
-rw-------   1 root       root       16549436 Mar 13 13:44 core
drwxr-xr-x   2 bin        bin             96 Aug  8  2000 sharedtape

localhost-root:/var/tombstones# file /var/adm/cmcluster/core
/var/adm/cmcluster/core:        core file from 'cmcld' - received SIGABRT

```


