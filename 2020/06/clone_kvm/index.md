# Clone KVM


## Procedures to clone virtual machine
<!--more-->
- List all virtual machine on system
    ```
    $ sudo virsh list --all
    ```

- Suspend virtual machine
    ```
    $ sudo virsh suspend main_node
    ```

- Clone virtual machine main_node to main_node_clone
    ```
    $ sudo virt-clone --connect qemu:///system --original main_node \
     --name main_node_clone --file /var/lib/libvirt/images/main_node_clone.qcow2
    ```

- Reset hostname and root user password.
    ```
    $ sudo virt-sysprep -d main_node_clone  --hostname main_node_clone --root-password password:123456
    ```

- List the operations supported by the virt-sysprep program.
    ```
    $ sudo virt-sysprep --list-operations
    abrt-data * Remove the crash data generated by ABRT
    backup-files * Remove editor backup files from the guest
    bash-history * Remove the bash history in the guest
    blkid-tab * Remove blkid tab in the guest
    ca-certificates   Remove CA certificates in the guest
    crash-data * Remove the crash data generated by kexec-tools
    cron-spool * Remove user at-jobs and cron-jobs
    customize * Customize the guest
    dhcp-client-state * Remove DHCP client leases
    dhcp-server-state * Remove DHCP server leases
    dovecot-data * Remove Dovecot (mail server) data
    firewall-rules   Remove the firewall rules
    flag-reconfiguration   Flag the system for reconfiguration
    fs-uuids   Change filesystem UUIDs
    kerberos-data   Remove Kerberos data in the guest
    logfiles * Remove many log files from the guest
    lvm-uuids * Change LVM2 PV and VG UUIDs
    machine-id * Remove the local machine ID
    mail-spool * Remove email from the local mail spool directory
    net-hostname * Remove HOSTNAME and DHCP_HOSTNAME in network interface configuration
    net-hwaddr * Remove HWADDR (hard-coded MAC address) configuration
    pacct-log * Remove the process accounting log files
    package-manager-cache * Remove package manager cache
    pam-data * Remove the PAM data in the guest
    passwd-backups * Remove /etc/passwd- and similar backup files
    puppet-data-log * Remove the data and log files of puppet
    rh-subscription-manager * Remove the RH subscription manager files
    rhn-systemid * Remove the RHN system ID
    rpm-db * Remove host-specific RPM database files
    samba-db-log * Remove the database and log files of Samba
    script * Run arbitrary scripts against the guest
    smolt-uuid * Remove the Smolt hardware UUID
    ssh-hostkeys * Remove the SSH host keys in the guest
    ssh-userdir * Remove ".ssh" directories in the guest
    sssd-db-log * Remove the database and log files of sssd
    tmp-files * Remove temporary files
    udev-persistent-net * Remove udev persistent net rules
    user-account   Remove the user accounts in the guest
    utmp * Remove the utmp file
    yum-uuid * Remove the yum UUID
    ```

- Resume virtual machine
    ```
    $ sudo virsh resume main_node
    ```

- Start a virtual machine
    ```
    $ sudo virsh start main_node_clone
    ```
#### Reference: [如何克隆一個KVM虛擬機並重置該虛擬機](https://huataihuang.gitbooks.io/cloud-atlas/virtual/kvm/startup/how_to_clone_a_kvm_virtual_machines_and_reset_the_vm.html)
