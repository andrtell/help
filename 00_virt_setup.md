# Linux hypervisor setup

`man virt-host-validate`

## Requirements

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

### Enable libvirt services

On _Void_ linux:
```
$ sudo ln -s /etc/sv/libvirtd /var/service/
$ sudo ln -s /etc/sv/virtlockd /var/service/
$ sudo ln -s /etc/sv/virtlogd /var/service/
```

On _SystemD_ linux:
```
$ sudo systemctl start libvirtd
$ sudo systemctl enable libvirtd
```

### Check requirements with libvirt installed
```
$ sudo virt-host-validate
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

### Test
```
$ virsh list --all
```
