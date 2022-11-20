# Virtualization VM Network

`man virsh`

## Enable dns lookup based of domain names

```
$ vi /etc/nsswitch.conf

    hosts: files libvirt_guest mdns dns
```

## Show VM network interfaces
```
$ virsh domifaddr ubuntu01

    Name       MAC address          Protocol     Address
    --------------------------------------------------------------
    vnet17     52:54:00:2d:5f:84    ipv4         192.168.122.78/24

$ virsh domiflist ubuntu01
 
    Interface   Type      Source    Model    MAC
    ----------------------------------------------------------
    vnet15      network   default   virtio   52:54:00:2d:5f:84
 
$ ssh user@ubuntu01

    user@ubuntu01:~$ ip a
        ...
        enp1s0
        link/ether 52:54:00:2d:5f:84
        inet 192.168.122.78/24
        ...
```

## Attach an interface
```
$ virsh attach-interface ubuntu01 network orange \
   --model virtio \
   --config \    # persist change
   --live        # running VM
```

## Detach an interface
```
$ virsh domiflist ubuntu01

   Interface   Type      Source    Model    MAC
   -------------------------------------------------------------
   vnet15      network   default   virtio   52:54:00:2d:5f:84
   vnet16      network   orange    virtio   52:54:00:f1:9a:06
    
$ virsh detach-interface ubunut01 network --mac 52:54:00:f1:9a:06 --config --live
```
