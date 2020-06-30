# Static IP assignment for KVM guest

## Procedures to assign static IP addresses for guest KVM
<!--more-->
###
The static IP of VM guest can be assigned by dnsmasq from VM host.
The configuration file location of dnsmasq can be found by checking the rum time parameter of dnsmasq process. The default value is **/var/lib/libvirt/dnsmasq/default.conf**
```
$ ps -ef | grep dnsmasq | grep -v grep
dnsmasq     6567       1  0 20:04 ?        00:00:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_leaseshelper
root        6568    6567  0 20:04 ?        00:00:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_leaseshelper
```
Get the name list of networks,
```
$ sudo virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes
```
Find out the guest VM’s MAC addresses, by
```
$ sudo virsh dumpxml {VM-NAME-HERE} | grep -i '<mac'
$ sudo virsh dumpxml main | grep -i '<mac'
      <mac address='52:54:00:21:cb:4e'/>
```

Note down the MAC addresses of the guet VM that you want to assign static IP addresses.

Edit the default network
```
$ sudo virsh net-edit default
```

Find the following section:
```
 <dhcp>
      <range start='192.168.122.100' end='192.168.122.254'/>
```

Append the static IP as follows after range:
```
     <host mac='52:54:00:21:cb:4e' name='main' ip='192.168.122.10'/>
```
Where,

    - mac='52:54:00:21:cb:4e' – VMs mac address
    - name='main' – VMs name.
    - ip='192.168.122.10' – VMs static IP.

Here is the complete file with three static DHCP entries for three VMs:
```
<network>
  <name>default</name>
  <uuid>5ac7cde2-2c09-4ef5-a038-9ffa1465e6c8</uuid>
  <forward mode='nat'/>
  <bridge name='virbr0' stp='on' delay='0'/>
  <mac address='52:54:00:d4:6d:5e'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.2' end='192.168.122.254'/>
      <host mac='52:54:00:21:cb:4e' name='main' ip='192.168.122.10'/>
      <host mac='52:54:00:16:19:19' name='worker1' ip='192.168.122.11'/>
      <host mac='52:54:00:a0:cc:12' name='worker2' ip='192.168.122.12'/>
    </dhcp>
  </ip>
</network>
```
Restart DHCP service:
```
$ sudo virsh net-destroy default
$ sudo virsh net-start default
```

Reference: 
- [KVM libvirt assign static guest IP addresses using DHCP on the virtual machine](https://www.cyberciti.biz/faq/linux-kvm-libvirt-dnsmasq-dhcp-static-ip-address-configuration-for-guest-os/)


