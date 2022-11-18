# Virtualization

## Vocab

domain => vm, guest

node => host, machine hosting virtual VM

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

If running task with a high cpu workload use 1 vCPU per 1 pCPU. In any case, the KVM hypervisor supports overcommitting virtualized CPUs (vCPUs). 

To check the max number of vCPUs that can be assign to a VM
```
$ virsh maxvcpus

# or

$ virsh domcapabilities | grep max
```

## Create a VM
```
$ virt-install \
  --name ubuntu01 \
  --memory 2048 \
  --vcpus 2 \
  --network network=default \
  --osinfo detect=on \
  --disk path=/some/path/ubuntu01.qcow2,size=8 \
  --cdrom /some/path/ubuntu-22.04.01-live-server-amd64.iso
```
## List VMs
```
$ virsh list --all
```
## Start & Stop VM

### Start a VM
```
$ virsh start ubuntu01
```

### Stop a VM gracefully (may never happen)
```
$ virsh shutdown ubuntu01
```

### Stop a VM un-gracefully
```
$ virsh destroy ubuntu01
```
## Remove a VM
```
$ virsh undefine ubuntu01 --remove-all-storage
```
## Show VM network ip
```
$ virsh domifaddr ubuntu01
```
## Networks

### List (virtual) networks
```
$ virsh net-list
```
### The default network

The default network is a NAT based virtual network. It also provides a DHCP server.

### Show network info
```
$ virsh net-info default
```
### Dump network info
```
$ virsh net-dumpxml default
```
### Network config location
```
$ ls /etc/libvirt/qemu/networks/*
```
## Create a new network
```
$ cat orange.xml

    <network>
      <name>orange</name>
    </network>

$ virsh net-define orange.xml

$ virsh net-dumpxml orange

    <network>
      <name>Orange</name>
      <uuid>1d567544-3ab7-499f-8621-fd2664c1e078</uuid>  
      <bridge name='virbr3' stp='on' delay='0'/>
      <mac address='52:54:00:81:f0:63'/>
    </network>
    
$ cat /etc/libvirt/qemu/networks/orange.xml
  
    # -""-
  
$ ip -o link | grep virbr3

    # nothing, network is not started
```

## Start a network
```
$ virsh net-start orange

$ ip -o link | grep virbr3

    virbr3 ...
```

## Show networks attached to domain
```
$ virsh domiflist ubuntu01
 
    Interface   Type      Source    Model    MAC
    ----------------------------------------------------------
    vnet15      network   default   virtio   52:54:00:2d:5f:84
 
$ ssh user@ubuntu01

    user@ubuntu01:~$ ip a

        enp1s0 ...
        link/ether 52:54:00:2d:5f:84
```
    
    
