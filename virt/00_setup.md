# Linux host as hypervisor running virtual machines and containers

## Network configuration

### Turning off network managers

For this to work, you need stop network-managers from running on boot.

To persist the setup, use something like `networkd` or write a script and run it manually.

### Setting up a bridge 

```
# ip link add name br0 type bridge
```

### Make physical ethernet interface slave of bridge

```
# ip link set dev enp58s0u1u4 master br0
```

### Bring interfaces up
```
# ip link set br0 up
# ip link set enp58s0u1u4 up
```

### Assign IP to bridge

At this point your interfaces should have no ip (check with `ip a`)

```
# dhcpcd br0
```

## Containers

We make a container visible on the network using a macvlan network (for fun and profit).

Note that this method is only available if running container as root.

```
# podman create network create \
  -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=br0 br0_net
```

One would expect that the local network DHCP server would be used, this is not the case. The container runtime (docker) will use it own DHCP server. From the local network point of view, an random static IP will be assign to the interface (with risk of collision). To solve this
add an ip-range that is excluded by the local network DHCP server, or covering IPs not assigned to any host on the local network.

```
# podman create network create \
  -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=br0 br0_net \
  --ip-range 192.168.1.254/32 # 1 ip range
  br0_net
```

Start a container using the network

```
# podman run -it --rm --replace --name ubuntu --network br0_net ubuntu
```

## Virtual machines

Create a new VM using the bridge to setup the VM network.

```
virt-install \
  --name ubuntu01 \
  --memory 2048 \
  --vcpus 2 \
  # --network network=default \
  --bridge br0
  --osinfo detect=on \
  --disk path=/some/path/ubuntu01.qcow2,size=8 \
  --cdrom /some/path/ubuntu-22.04.01-live-server-amd64.iso
```

Your VM should be visible on the network after creation with its own IP assigned by the local network DHCP server.
