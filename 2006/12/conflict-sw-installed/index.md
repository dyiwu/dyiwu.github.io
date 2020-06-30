# Application fail to start with 'bind address failure' error message.

## Application fail to start with 'bind address failure' error message.

```

Symptom
-------
  Application fail to start with "bind address failure" 
  error message.

Root Cause
----------
  Conflict software product ACC/X25 be installed on system.

Action Plan
-----------
  Remove the unsutable ACC/X25 software product by using swremove command.

Detail Analysis
---------------
  There are two type of X25 card supported on hpux system.
  Those are PSI J2815AR card and ACC card.

  EDIlink applicaton fail to start with the following messages.
  
#cat /var/adm/sw/swremove.log

=======  12/27/06 17:54:27 EAT  BEGIN swremove SESSION (interactive)

NOTE:    The interactive UI was invoked, since no software was
         specified.
         
       * Session started for user "root@localhost".
         
       * The interactive UI failed to start.  If you wish to run
         non-interactively, you must specify some software as part of
         the swremove command.

=======  12/27/06 17:54:31 EAT  END swremove SESSION (interactive)


=======  12/27/06 17:55:08 EAT  BEGIN swremove SESSION (interactive)

NOTE:    The interactive UI was invoked, since no software was
         specified.
         
       * Session started for user "root@localhost".
         
       * The interactive UI failed to start.  If you wish to run
         non-interactively, you must specify some software as part of
         the swremove command.

=======  12/27/06 17:55:13 EAT  END swremove SESSION (interactive)


=======  12/27/06 17:55:39 EAT  BEGIN swremove SESSION (interactive)

NOTE:    The interactive UI was invoked, since no software was
         specified.
         
       * Session started for user "root@localhost".
         
       * Beginning Selection
             admin_directory            /var/adm/sw
             agent_auto_exit            false
             agent_timeout_minutes      10000
             allow_split_patches        false
             auto_kernel_build          true
             autoreboot                 false
             autoremove_job             true
             autoselect_dependents      false
             autoselect_reference_bundles       true
             compress_index             false
             customer_umask_octal       0022
             distribution_target_directory      /var/spool/sw
             enforce_dependencies       true
             enforce_kernbld_failure    true
             enforce_scripts            true
             force_single_target        false
             installed_software_catalog products
             log_msgid                  0
             logdetail                  false
             logfile                    /var/adm/sw/swremove.log
             loglevel                   1
             mount_all_filesystems      true
             polling_interval           2
             preview                    false
             remove_empty_depot         true
             reuse_short_job_numbers    true
             rpc_binding_info           ncacn_ip_tcp:[2121] ncadg_ip_udp:[2121]
             rpc_timeout                5
             run_as_superuser           true
             run_scripts                true
             select_local               true
             software_view              all_bundles
             verbose                    1
             write_remote_files         false
       * Source:                 localhost:/
       * Targets:                localhost:/
       * Software selections:
             ACC.ACC-FW,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=B.03.40.0
3,fa=HP-UX_B.11.23_IA
             ACC.ACC-KRN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=B.03.40.
03,fa=HP-UX_B.11.23_IA
             ACC.ACC-MAN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=B.03.40.
03,fa=HP-UX_B.11.23_IA/PA
             ACC.ACC-RUN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=B.03.40.
03,fa=HP-UX_B.11.23_IA
             ACC.ACC-X25AN-RUN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=B.
03.40.03,fa=HP-UX_B.11.23_IA
             ACC-DEV.ACC-PRG,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=B.03
.40.03,fa=HP-UX_B.11.23_IA
             ACC-HDLC.ACC-HDLC-FW,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr
=B.03.40.03,fa=HP-UX_B.11.23_IA
             ACC-HDLCNRM.ACC-HDLCNRM-FW,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v
=HP,fr=B.03.40.03,fa=HP-UX_B.11.23_IA/PA
             ACC-LAPD.ACC-LAPD-FW,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr
=B.03.40.03,fa=HP-UX_B.11.23_IA
             ACC-SX25.ACC-SX25-KRN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,f
r=B.03.40.03,fa=HP-UX_B.11.23_IA
             ACC-SX25.ACC-SX25-RUN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,f
r=B.03.40.03,fa=HP-UX_B.11.23_IA
             ACC-X25.ACC-X25-FW,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=B
.03.40.03,fa=HP-UX_B.11.23_IA
             ACC-X25.ACC-X25-KRN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=
B.03.40.03,fa=HP-UX_B.11.23_IA
             ACC-X25.ACC-X25-RUN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP,fr=
B.03.40.03,fa=HP-UX_B.11.23_IA
             ACC-X25-DEV.ACC-X25-MAN,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP
,fr=B.03.40.03,fa=HP-UX_B.11.23_IA/PA
             ACC-X25-DEV.ACC-X25-PRG,l=/,r=B.03.40.03,a=HP-UX_B.11.23_IA/PA,v=HP
,fr=B.03.40.03,fa=HP-UX_B.11.23_IA
       * Beginning Analysis
       * The analysis phase succeeded for "localhost:/".
       * Ending Analysis
       * Before starting Removal, you should be aware of the following:
         
         The kernel will be re-built on the local system.
         
         The system will be rebooted as soon as Removal is complete.
         
         Do you still wish to start Removal?
       * Beginning Task Execution
       * Proceeding with Task Execution on the following targets:
       * localhost:/
       * The execution phase succeeded for "localhost:/".
       * Ending Task Execution
       * Your local system will be rebooted when you press "OK" in this
         window.  Check the logfile "/var/adm/sw/swagent.log" after
         reboot to see if there were any software configuration
         problems.

=======  12/27/06 18:01:27 EAT  END swremove SESSION (interactive)


=======  12/28/06 09:14:16 EAT  BEGIN swremove SESSION (interactive)

NOTE:    The interactive UI was invoked, since no software was
         specified.
         
       * Session started for user "root@localhost".
         
       * Beginning Selection
             admin_directory            /var/adm/sw
             agent_auto_exit            false
             agent_timeout_minutes      10000
             allow_split_patches        false
             auto_kernel_build          true
             autoreboot                 false
             autoremove_job             true
             autoselect_dependents      false
             autoselect_reference_bundles       true
             compress_index             false
             customer_umask_octal       0022
             distribution_target_directory      /var/spool/sw
             enforce_dependencies       true
             enforce_kernbld_failure    true
             enforce_scripts            true
             force_single_target        false
             installed_software_catalog products
             log_msgid                  0
             logdetail                  false
             logfile                    /var/adm/sw/swremove.log
             loglevel                   1
             mount_all_filesystems      true
             polling_interval           2
             preview                    false
             remove_empty_depot         true
             reuse_short_job_numbers    true
             rpc_binding_info           ncacn_ip_tcp:[2121] ncadg_ip_udp:[2121]
             rpc_timeout                5
             run_as_superuser           true
             run_scripts                true
             select_local               true
             software_view              all_bundles
             verbose                    1
             write_remote_files         false
       * Source:                 localhost:/
       
       
[localhost:/home/edilink/logs] #tail *061225
tail *061225
1      06/12/25 15:02:55 [error] initial x.25 server failure
1      06/12/25 15:02:55 [notice] edilink system stop
1      06/12/25 15:03:06 [notice] edilink system start
1      06/12/25 15:03:06 [error] x.25 server bind address failure:Address alread
y in use
1      06/12/25 15:03:06 [error] initial x.25 server failure
1      06/12/25 15:03:06 [notice] edilink system stop
1      06/12/25 15:14:17 [notice] edilink system start
1      06/12/25 15:14:17 [error] x.25 server bind address failure:Address alread
y in use
1      06/12/25 15:14:17 [error] initial x.25 server failure
1      06/12/25 15:14:17 [notice] edilink system stop

```

