# Marvell Yukon 2 Gigabit Ethernet driver on Centos 8.1


<!--more-->

- Check the PCI-E interface on system by:
    ```
    $ lspci |grep net
    45:00.0 Ethernet controller: Marvell Technology Group Ltd. 
    88E8072 PCI-E Gigabit Ethernet Controller (rev 10)
    ```

- [Package description](https://centos.pkgs.org/8/elrepo-x86_64/kmod-sky2-1.30-2.el8_1.elrepo.x86_64.rpm.html)
- [Binary package](https://mirror.rackspace.com/elrepo/elrepo/el8/x86_64/RPMS/kmod-sky2-1.30-2.el8_1.elrepo.x86_64.rpm)

- Download latest driver rpm package file from elrepo-release
    ```
    # wget https://mirror.rackspace.com/elrepo/elrepo/el8/x86_64/RPMS/kmod-sky2-1.30-2.el8_1.elrepo.x86_64.rpm
    ```
- Install it
    ```
    # rpm -Uvh kmod-sky2-1.30-2.el8_1.elrepo.x86_64.rpm
    ```
- Install kmod-sky2 rpm package:
    ```
    # dnf install kmod-sky2
    ```

