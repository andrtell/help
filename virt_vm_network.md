# Virtualization VM Network

## Show VM network ip
```
$ virsh domifaddr ubuntu01
```

## Show VM network interfaces
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

### Attach an interface to a VM
```
$ virsh attach-interface ubuntu01 network orange \
  --model virtio \
  --config \    # persist change, survives VM restart
  --live        # attach interface to currently running VM
```

### Detach an interface from a domain
```
$ virsh domiflist ubuntu01

   Interface   Type      Source    Model    MAC
   -------------------------------------------------------------
   vnet15      network   default   virtio   52:54:00:2d:5f:84
   vnet16      network   orange    virtio   52:54:00:f1:9a:06
    
$ virsh detach-interface ubunut01 network --mac 52:54:00:f1:9a:06 --config --live
```
