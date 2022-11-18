# Virtualization

## Requirements & Setup

### Check if CPU supports virtualization (Intel, AMD)
```
$ lscpu # then inspect

# or

$ grep --color -Ew 'svm|vmx|lm' /proc/cpuinfo # vmx = Intel VT-x, svm = AMD-V, lm = 64 bit
```
### Check to see if KVM kernel modules are loaded
```
$ lsmod | grep kvm
```

### Install required packages (more or less the same on different distros)
```
$ sudo pkg install qemu libvirt virt-manager virt-manager-tools 
```

### Check requirements with libvirt installed
```
$ sudo virt-host-validate
```

### Start/Enable libvirt services
```
# Ubuntu
$ sudo systemctl start libvirtd
$ sudo systemctl enable libvirtd

# Void
$ sudo ln -s /etc/sv/libvirtd /var/service/
$ sudo ln -s /etc/sv/virtlockd /var/service/
$ sudo ln -s /etc/sv/virtlogd /var/service/
```

### Run virsh without sudo

(group `libvirt` may differ between distros)

```
$ sudo usermod -aG libvirt myuser
```

### Change libvity default url 

The url `qemu::///user` is the default.

```
$ echo 'uri_default = "qemu:///system"' >> ~/.config/libvirt/libvirt.conf
```

## Info

### Check what can be assigned to a VM
```
$ virsh domcapablities

$ virsh domcapabilites | grep diskDevice -A 7
```

### Number of vCPUs

If running task with a high cpu workload use 1 vCPU per 1 pCPU. In any case, the KVM hypervisor 
supports overcommitting virtualized CPUs (vCPUs). 

To check the max number of vCPUs that can be assign to a VM
```
$ virsh maxvcpus

# or

$ virsh domcapabilities | grep max
```