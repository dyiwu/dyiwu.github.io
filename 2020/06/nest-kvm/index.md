# Enabling nested virtualization in KVM

Nested virtualization allows you to run a virtual machine (VM) inside another VM
 while still using hardware acceleration from the host.
<!--more-->

## Checking if nested virtualization is supported
For Intel processors, check the /sys/module/kvm_intel/parameters/nested file.  
For AMD processors, check the /sys/module/kvm_amd/parameters/nested file.  
If you see 1 or Y, nested virtualization is supported; if you see 0 or N, nested virtualization is not supported.

For example:
```
$ cat /sys/module/kvm_intel/parameters/nested
Y
```
## Enabling nested virtualization
To enable nested virtualization for Intel processors:
Shut down all running VMs and unload the kvm_probe module:
```
# modprobe -r kvm_intel
Activate the nesting feature:
# modprobe kvm_intel nested=1
```
Nested virtualization is enabled until the host is rebooted. To enable it permanently, add the following line to the /etc/modprobe.d/kvm.conf file:
```
options kvm_intel nested=1
```
## Configuring nested virtualization in virt-manager
Configure your VM to use nested virtualization:  
Open virt-manager, double-click the VM in which you wish to enable nested virtualization, and click the Show virtual hardware details icon.

Click CPUs in the side menu. In the Configuration section, there are two options - either type host-passthrough in the Model: field, or select the Copy host CPU configuration check box (that fills the host-model value in the Model field).

Using host-passthrough is not recommended for general usage. It should only be used for nested virtualization purposes.
Click Apply.

## Testing nested virtualization
Start the virtual machine.  
On the virtual machine, run:
```
# dnf group install virtualization
Verify that the virtual machine has virtualization correctly set up:

# virt-host-validate
  QEMU: Checking for hardware virtualization                                 : PASS
  QEMU: Checking for device /dev/kvm                                         : PASS
  QEMU: Checking for device /dev/vhost-net                                   : PASS
  QEMU: Checking for device /dev/net/tun                                     : PASS
  LXC: Checking for Linux >= 2.6.26           
```
#### Reference: [How to enable nested virtualization in KVM](https://docs.fedoraproject.org/en-US/quick-docs/using-nested-virtualization-in-kvm/index.html)


