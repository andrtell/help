# Virtualization Network

`man virsh` 

## List (virtual) networks
```
$ virsh net-list
```

## Show network info
```
$ virsh net-info default
```

## Dump network info
```
$ virsh net-dumpxml default
```

## Network config location
```
$ ls /etc/libvirt/qemu/networks/*
```

## Create a new (isolated) network
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

## Edit network 
```
$ virsh net-destroy banana # first stop it

$ virsh net-edit banana
```

## Start a network
```
$ virsh net-start orange

$ ip -o link | grep virbr3

    virbr3 ...
```

## Stop a network
```
$ virsh net-destroy orange
```

## Create a routed network

A routed network uses the routing table of the hypervisor. 

You will have to set up SNAT and/or DNAT on the hypervisor your self.

One advantage is that if the hypervisor is set up as a gateway for some other network, hosts
on those networks will be able to communicate with the VMs.

```
$ cat banana.xml

    <network>
      <name>banana</name>
      <forward dev='wlp0s20f3' mode='route'>
        <interface dev='wlp0s20f3'/>
      </forward>
      <ip address='192.168.77.1' netmask='255.255.255.0'>
      </ip>
    </network>

$ virsh net-define banana.xml

$ virsh net-start banana
```

## Create a network with SNAT

Same as routed but sets up SNAT on the hypervisor (for bridge network) using iptables

In this example we are adding DHCP aswell.

```
$ cat melon.xml

    <network>
      <name>melon</name>
      <forward mode='nat'>
        <nat>
          <port start='1024' end='65535' />
        </nat>
      </forward>
      <ip address='192.168.88.1' netmask='255.255.255.0'>
        <dhcp>
          <range start='192.168.88.2' end='192.168.88.254'/>
        </dhcp>
      </ip>
    </network>
    
$ virsh net-define melon.xml

$ virsh net-start melon
```
