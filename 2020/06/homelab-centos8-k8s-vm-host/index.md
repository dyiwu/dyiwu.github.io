# Home Lab - K8s on Centos 8 - VM host

This home lab is going to build a CentOS 8 VM host on bare matel and 3 VM guest nodes be form up as a k8s cluster. The first step is going to craete the VM host on the bare metal.

<!--more-->
### **VM host base environment**
- Select ***Server with GUI*** as base environment for this Centos 8 VM host with following configuration. 

    - Intel(R) Core(TM) i7 CPU       Q 720  @ 1.60GHz
    - 8 GB memory
    - Disk partitions 
    ```
    # fdisk -l  /dev/sda
    Disk /dev/sda: 698.7 GiB, 750156374016 bytes, 1465149168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0xc2d42c2b

    Device     Boot    Start        End    Sectors   Size Id Type
    /dev/sda1  *        2048    4196351    4194304     2G 83 Linux
    /dev/sda2        4196352   37750783   33554432    16G 82 Linux swap / Solaris
    /dev/sda3       37750784 1465147391 1427396608 680.7G 83 Linux

    # df -TH|grep -e Size -e sda
    Filesystem     Type      Size  Used Avail Use% Mounted on
    /dev/sda3      xfs       731G   11G  721G   2% /
    /dev/sda1      ext4      2.1G  214M  1.8G  12% /boot

    # cat /proc/swaps
    Filename                Type            Size    Used    Priority
    /dev/sda2               partition       16777212        0       -2

    ```
    - Anaconda packages
    ```
    %packages
    @^graphical-server-environment
    @development
    @graphical-admin-tools
    @headless-management
    @remote-system-management
    @system-tools
    @virtualization-client
    @virtualization-hypervisor
    @virtualization-tools

    %end
    ```
 
#### Reference: [How to Install CentOS 8](https://linoxide.com/distros/how-to-install-centos/)

### **Enable Community Enterprise Linux Repository and extra packages**

- Download latest elrepo-release rpm from
    ```
    # wget https://mirror.rackspace.com/elrepo/extras/el8/x86_64/RPMS/elrepo-release-8.2-1.el8.elrepo.noarch.rpm
    ```

- Install elrepo-release rpm
    ```
    # rpm -Uvh elrepo-release-8.2-1.el8.elrepo.noarch.rpm
    ```

- Install elrepo-release rpm package:
    ```
    # dnf --enablerepo=elrepo-extras install elrepo-release
    ```

### **Repository list**
- ***dnf repolist*** command can be used to list all enabled repositories. Provides more detailed information when -v option is used.

    ```
    # dnf repolist
    Last metadata expiration check: 0:17:47 ago on Sat 16 May 2020 12:06:02 PM CST.
    repo id                repo name                                                        status
    AppStream              CentOS-8 - AppStream                                             5,318
    BaseOS                 CentOS-8 - Base                                                  1,661
    elrepo                 ELRepo.org Community Enterprise Linux Repository - el8              90
    extras                 CentOS-8 - Extras                                                   16
    google-chrome          google-chrome      

    # dnf repolist -v
    ```
### **Google Chrome**

- Download the latest Chrome 64-bit .rpm package
    ```
    # wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
    ```
-  Install Chrome Browser
    ```
    # sudo dnf localinstall google-chrome-stable_current_x86_64.rpm
    ```
- Starting Chrome Browser
    Now that Chrome Browser is installed on your CentOS system, you can launch it either from the command line by typing google-chrome & or by clicking on the Chrome icon (Activities -> Chrome Browser).

- Updating Chrome Browser
    During the package installation, the official Google repository will be added to your system. Use the following cat command to verify that the file exists:
    ```
    # cat /etc/yum.repos.d/google-chrome.repo
    [google-chrome]
    name=google-chrome
    baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
    enabled=1
    gpgcheck=1
    gpgkey=https://dl.google.com/linux/linux_signing_key.pub
    ```
    When a new version is released, you can perform an update with dnf or through your desktop standard Software Update tool.

#### Reference：[How to Install Google Chrome Web Browser on CentOS 8](https://linuxize.com/post/how-to-install-google-chrome-web-browser-on-centos-8/)

### **br0 Network Bridge** (optional)
- Check connections.
    ```
    $ sudo nmcli connection show 
    NAME                UUID                                  TYPE      DEVICE
    Wired connection 1  492d853c-ab48-4073-9f57-c781b024d0d0  ethernet  ens1
    DrayTek2920n 1      c4c685cf-e26f-4694-927c-e5c87213a445  wifi      wlp68s0b1
    virbr0              52d52d47-a289-4beb-95bf-b9749e78522f  bridge    virbr0
    ```
- Bridge Network Configuration
    ```
    # cat /etc/sysconfig/network-scripts/ifcfg-br0
    DEVICE=br0
    TYPE=Bridge
    NAME=br0
    DELAY=0
    STP=off
    ONBOOT=yes
    IPADDR=192.168.1.120
    NETMASK=255.255.255.0
    GATEWAY=192.168.1.1
    BOOTPROTO=none
    DEFROUTE=yes
    NM_CONTROLLED=yes
    IPV6INIT=no
    DNS1=192.168.1.1
    DNS2=168.95.192.1
    DNS3=8.8.8.8
    ```
- Interface configuration
    ```
    # cat /etc/sysconfig/network-scripts/ifcfg-ens1
    DEVICE=ens1
    TYPE=Ethernet
    NAME=ens1
    ONBOOT=yes
    BRIDGE=br0
    UUID=492d853c-ab48-4073-9f57-c781b024d0d0
    ```
- Reboot system to confirm that bridging is working.
    ```
    # reboot
    ```
- Once rebooted, verify the settings.
    ```
    # nmcli connection show
    NAME            UUID                                  TYPE      DEVICE
    br0             d2d68553-f97e-7549-7a26-b34a26f29318  bridge    br0
    DrayTek2920n 1  c4c685cf-e26f-4694-927c-e5c87213a445  wifi      wlp68s0b1
    virbr0          e34ecdf4-6be2-442c-b37a-54e4950e5999  bridge    virbr0
    ens1            492d853c-ab48-4073-9f57-c781b024d0d0  ethernet  ens1

    # ifconfig ens1
    ens1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        ether 70:5a:b6:9a:52:57  txqueuelen 1000  (Ethernet)
        RX packets 3483  bytes 293433 (286.5 KiB)
        RX errors 0  dropped 7  overruns 0  frame 0
        TX packets 628  bytes 69522 (67.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 17

    # ifconfig br0
    br0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.120  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::725a:b6ff:fe9a:5257  prefixlen 64  scopeid 0x20<link>
        ether 70:5a:b6:9a:52:57  txqueuelen 1000  (Ethernet)
        RX packets 3354  bytes 204810 (200.0 KiB)
        RX errors 0  dropped 1616  overruns 0  frame 0
        TX packets 640  bytes 70750 (69.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ```
#### Reference: [How To Create a Linux Network Bridge on RHEL 8 / CentOS 8](https://computingforgeeks.com/how-to-create-a-linux-network-bridge-on-rhel-centos-8/)

### **Install KVM**
In here, we will like to go through the steps to install the latest release of KVM hypervisor on Centos 8. This will include the installation of KVM management tools – libguestfs-tools.
- Ensure host CPU has Intel VT or AMD-V Virtualization extensions
    ```
    # cat /proc/cpuinfo | egrep "vmx|svm"|head -1
    flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm sse4_1 sse4_2 popcnt lahf_lm pti ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid dtherm ida flush_l1d

    # lscpu | grep Virtualization
    Virtualization:      VT-x
    ```
- Install KVM / QEMU on RHEL/ CentOS 8
KVM packages are distributed on RHEL 8 via AppStream repository. Install KVM on your RHEL 8 server by running the following commands:
    ```
    # yum update
    # yum install @virt
    ```
- After installation, verify that Kernel modules are loaded.
    ```
    # lsmod | grep kvm
    kvm_intel             290816  0
    kvm                   761856  1 kvm_intel
    irqbypass              16384  1 kvm
    ```
- Also install useful tools for virtual machine management.
    ```
    # dnf -y install virt-top libguestfs-tools
    ```
- Start and enable KVM daemon
By default, KVM daemon libvirtd is not started, start the service using the command:
    ```
    # systemctl enable --now libvirtd
    # systemctl status libvirtd.service
    ● libvirtd.service - Virtualization daemon
       Loaded: loaded (/usr/lib/systemd/system/libvirtd.service; enabled; vendor preset: enabled)
       Active: active (running) since Sat 2020-05-16 08:17:51 CST; 1h 28min ago
         Docs: man:libvirtd(8)
               https://libvirt.org
     Main PID: 1365 (libvirtd)
        Tasks: 17 (limit: 32768)
       Memory: 51.4M
           CGroup: /system.slice/libvirtd.service
               └─1365 /usr/sbin/libvirtd
    ```
- Install Virtual machine Manager GUI – Optional
Install the virt-manager tool which allows us to manage Virtual Machines from a GUI.
    ```
    # yum -y install virt-manager
    ```

#### Reference: [How To Install KVM on RHEL 8 / CentOS 8 Linux](https://computingforgeeks.com/how-to-install-kvm-on-rhel-8/)

### **Using libvirtd but don't want virbr0** (optional)
On a Linux host server, the virtual network switch shows up as a network interface. The default one, created when the libvirt daemon is first installed and started, shows up as virbr0. It will act as a gateway for the VMs to route traffic. libvirtd will also insert iptables rules in iptable configuration for proper routing/natting of VM packets.

If we don't want to use libvirtd service, we can stop the same which will remove all these network configurations from the system for virbr0 interface.

But in our case scenario, libvirtd service will be kept running regarding this bare metal system is acting as KVM host but we have br0 setup to let guest VMs use it, so we will remove the virbr0 interface. Follow the steps below to remove the virbr0 interface.

- List the default network set-up for the virtual machines
    ```
    # virsh net-list
     Name                 State      Autostart     Persistent
    ----------------------------------------------------------
     default              active     yes           yes
    ```
- Destroy the network default
    ```
    # virsh net-destroy default
    Network default destroyed
    ```
- Permanently remove the default vitual network from the configuration.
    ```
    # virsh net-undefine default
    Network default has been undefined
    ```
- Verify it in the ifconfig or ip command output.
    ```
    # ifconfig virbr0
    virbr0: error fetching interface information: Device not found

    # nmcli connection show
    NAME            UUID                                  TYPE      DEVICE
    br0             d2d68553-f97e-7549-7a26-b34a26f29318  bridge    br0
    DrayTek2920n 1  c4c685cf-e26f-4694-927c-e5c87213a445  wifi      wlp68s0b1
    ens1            492d853c-ab48-4073-9f57-c781b024d0d0  ethernet  ens1
    ```
#### Reference: [How to Remove virbr0 and lxcbr0 Interfaces on CentOS/RHEL 6,7](https://www.thegeekdiary.com/how-to-remove-virbr0-and-lxcbr0-interfaces-on-centos-rhel-5-and-rhel-7/)

### **Xrdp service**
Xrdp is an open-source implementation of the Microsoft Remote Desktop Protocol (RDP) that allows you to graphically control a remote system. With RDP, you can log in to the remote machine and create a real desktop session the same as if you had logged in to a local machine.

- Installing Xrdp
   Xrdp is available in the EPEL software repository. If EPEL is not enabled on your system, enable it by typing:
    ```
    # dnf install epel-release
    ```
- Install the Xrdp package:
    ```
    # dnf install xrdp 
    ```
- Start up and enable at boot
    When the installation process is complete, start the Xrdp service and enable it at boot:
    ```
    # systemctl enable xrdp --now
    ```
- Verify the xrdp status
    ```
    # systemctl status xrdp
    ● xrdp.service - xrdp daemon
       Loaded: loaded (/usr/lib/systemd/system/xrdp.service; enabled; vendor preset: disabled)
       Active: active (running) since Sat 2020-05-16 13:31:09 CST; 12s ago
         Docs: man:xrdp(8)
               man:xrdp.ini(5)
     Main PID: 9174 (xrdp)
        Tasks: 1 (limit: 26213)
       Memory: 1.0M
       CGroup: /system.slice/xrdp.service
               └─9174 /usr/sbin/xrdp --nodaemon
    ```
- Configuring xrdp

    The configuration files are located in the /etc/xrdp directory. For basic Xrdp connections, you do not need to make any changes to the configuration files. Xrdp uses the default X Window desktop, which in this case, is Gnome.

    The main configuration file is named xrdp.ini. This file is divided into sections and allows you to set global configuration settings such as security and listening addresses and create different xrdp login sessions.

    Whenever you make any changes to the configuration file you need to restart the Xrdp service:
    ```
    # systemctl restart xrdp
    ```
    Xrdp uses startwm.sh file to launch the X session. If you want to use another X Window desktop, edit this file.

- Configuring Firewall

    By default, xrdp listens on port 3389 on all interfaces. If you run a firewall on your CentOS machine (which you should always do), you’ll need to add a rule to allow traffic on the Xrdp port.

    Typically you would want to allow access to the Xrdp server only from a specific IP address or IP range. For example, to allow connections only from the 192.168.1.0/24 range, enter the following command:
    ```
    # firewall-cmd --new-zone=xrdp --permanent
    # firewall-cmd --zone=xrdp --add-port=3389/tcp --permanent
    # firewall-cmd --zone=xrdp --add-source=192.168.1.0/24 --permanent
    # firewall-cmd --reload
    ```
    To allow traffic to port 3389 from anywhere use the commands below. Allowing access from anywhere is highly discouraged for security reasons.
    ```
    # firewall-cmd --add-port=3389/tcp --permanent
    # firewall-cmd --reload
    ```
    For increased security, you may consider setting up Xrdp to listen only on localhost and creating an SSH tunnel that securely forwards traffic from your local machine on port 3389 to the server on the same port.

    Another secure option is to install OpenVPN and connect to the Xrdp server trough the private network.

- Connecting to the Xrdp Server

    Now that the Xrdp server is configured, it is time to open your local Xrdp client and connect to the remote CentOS 8 system.

    Windows users can use the default RDP client. Type “remote” in the Windows search bar and click on “Remote Desktop Connection”. This will open up the RDP client. In the “Computer” field, type the remote server IP address and click “Connect”.

    On the login screen, enter your username and password and click “OK”.
    Once logged in, you should see the default Gnome desktop. You can now start interacting with the remote desktop from your local machine using your keyboard and mouse.

#### Reference: [How to Install Xrdp Server (Remote Desktop) on CentOS 8](https://linuxize.com/post/how-to-install-xrdp-on-centos-8/)

### **Cockpit**
The Cockpit is a web console with an easy to use web-based interface that enables you to carry out administrative tasks on your servers. Also being a web console, it means you can also access it through a mobile device as well.

- Installing Cockpit Web Console
    ```
    # yum install cockpit
    ```
- Enable and start the ***cockpit.service***
    ```
    # systemctl start cockpit.socket
    # systemctl enable --now cockpit.socket
    # systemctl status cockpit.socket
    ● cockpit.socket - Cockpit Web Service Socket
       Loaded: loaded (/usr/lib/systemd/system/cockpit.socket; enabled; vendor preset: disabled)
       Active: active (listening) since Sat 2020-05-16 14:04:31 CST; 23s ago
         Docs: man:cockpit-ws(8)
       Listen: [::]:9090 (Stream)
        Tasks: 0 (limit: 26213)
       Memory: 1.1M
       CGroup: /system.slice/cockpit.socket
    # ps auxf|grep cockpit
    root     14317  0.0  0.0  12108  1072 pts/0    S+   14:20   0:00  |           \_ grep --color=auto cockpit
    cockpit+ 12427  0.2  0.1 460256 11776 ?        Ssl  14:13   0:00 /usr/libexec/cockpit-ws
    root     12436  0.0  0.0 135688  6036 ?        S    14:13   0:00  \_ /usr/libexec/cockpit-session localhost
    dyiwu    12464  0.3  0.1 414680 14672 ?        Sl   14:13   0:01      \_ cockpit-bridge
    ```
- open the cockpit port 9090 in the firewall
    ```
    # firewall-cmd --add-service=cockpit --permanent
    # firewall-cmd --reload
    ```
- Open the Cockpit web console in your web browser at the following URL’s:
    ```
    Locally: https://localhost:9090
    Remotely with the server’s hostname: https://example.com:9090
    Remotely with the server’s IP address: https://192.168.1.120:9090
    ```
    The console calls a certificate from the /etc/cockpit/ws-certs.d directory and uses the .cert extension file. To avoid having to prompt security warnings, install a certificate signed by a certificate authority (CA).

    In the web console login screen, enter your system user name and password.

#### Reference:
- [How to Install Cockpit Web Console in CentOS 8](https://www.tecmint.com/install-cockpit-web-console-in-centos-8/)
- [cockpit project](https://cockpit-project.org/)
- [cockpit with Virtual Machines](https://cockpit-project.org/guide/latest/feature-virtualmachines.html#feature-virtualmachines-systemaccess)

### **Chinese input method**
- Locate the ibus pinyin package.
    ```
    # dnf search ibus*
    ```
- Install ibus-libpinyin.x86_64 package
    ```
    # dnf install ibus-libpinyin.x86_64
    ```

- Install ibus-libzhuyin.x86_64 package optional.
   ```
   # dnf install ibus-libzhuyin.x86_64
   ```
- Reboot system after package installed.

- Now, going to the Settings —> Region & Language —> Input Sources, click the add botton to have chinese input method.

### **Lid close event handler**
By default, closing lid will suspend laptop. Set HandleLidSwitch and HandleLidSwitchDocked to "ignore" in /etc/systemd/logind.conf to ignore the lid closing event.

```
$ sudo vi /etc/systemd/logind.conf

HandleLidSwitch=ignore        <---- Set both of these
HandleLidSwitchDocked=ignore  <---- to ignore lid events.

$ sudo systemctl restart systemd-logind
```

-------

- [virsh commands cheatsheet to manage KVM guest virtual machines](https://computingforgeeks.com/virsh-commands-cheatsheet/)

- [How to Install a Kubernetes Cluster on CentOS 8](https://www.tecmint.com/install-a-kubernetes-cluster-on-centos-8/)


