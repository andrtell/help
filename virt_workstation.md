# Workstation setup

Setup host such that VMs and containers become visible hosts on the local network.

## Network setup.

Manual setup of virtual bridge

```
$ sudo ip link add name br0 type bridge
$ sudo ip link set dev enp58s0u1u4 master br0

$ sudo ip link set br0 up
$ sudo ip link set enp58s0u1u4 up

$ sudo dhcpcd br0

$ ip a
```

Automatic setup on _Void_ linux:

```
$ cat /etc/rc.local

    ip link add name br0 type bridge
    ip link set dev enp58s0u1u4 master br0

    ip link set br0 up
    ip link set enp58s0u1u4 up
```

Make sure dhcpcd only runs on specific interfaces:

(change interfaces to match your setup `ip link`)

```
$ sudo cp -R /etc/sv/dhcpcd-eth0 /etc/sv/dhcpcd-wlp0s20f3
$ sudo sed -i 's/eth0/wlp0s20f3' /etc/sv/dhcpcd-wlp0s20f3/run
$ sudo ln -s /etc/sv/dhcpcd-wlp0s20f3 /var/service/

$ sudo cp -R /etc/sv/dhcpcd-eth0 /etc/sv/dhcpcd-br0
$ sudo sed -i 's/eth0/br0' /etc/sv/dhcpcd-br0/run
$ sudo ln -s /etc/sv/dhcpcd-br0 /var/service/
```

Automatic setup on _Systemd_ linux:

```
$ cat /etc/systemd/network/br.netdev

    [NetDev]
    Name=br0
    Kind=bridge

$ cat /etc/systemd/network/001-br0-bind.network

    [Match]
    Name=eno1

    [Network]
    Bridge=br0

$ cat /etc/systemd/network/002-br0-dhcp.network

    [Match]
    Name=br0

    [Network]
    DHCP=ipv4
```

Then

```
$ systemctl enable systemd-networkd
```

## Start a VM

You should now be able to create a new VM using the bridge.

```
$ virt-install \
  --name ubuntu01 \
  --memory 2048 \
  --vcpus 2 \
  --bridge br0 # <-
  --osinfo detect=on \
  --disk path=/some/path/ubuntu01.qcow2,size=8 \
  --cdrom /some/path/ubuntu-22.04.01-live-server-amd64.iso
```

The VM should be visible on the network with its own MAC-address and IP assigned by the local network DHCP server.

## Containers

We make a container visible on the network using a macvlan network.

_Note: this method is only available when running rootfull continers._

```
$ sudo podman create network create \
  -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=br0 br0_net
```

One would expect that the local network DHCP server would be used, this is not the case. 

From the local network point of view, an random static IP will be assign to the interface (risk of collision!). 

One way to solve this is to add an ip-range that is not used by the local network DHCP server, or using a range with IPs not assigned to any host on the local network.

```
$ sudo podman create network create \
  -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=br0 br0_net \
  --ip-range 192.168.1.254/32 # 1 ip range
  br0_net
```

Inspect the new network

```
$ sudo podman network inspect br0_net

[
     {
          "name": "br0_net",
          "driver": "macvlan",
          "network_interface": "br0",
          "subnets": [
               {
                    "subnet": "192.168.1.0/24",
                    "gateway": "192.168.1.1"
               }
          ],
          "ipam_options": {
               "driver": "host-local"
          }
     }
]
```

Another way to solve this is by starting the cni dhcp daemon

https://xaviercovis.github.io/IP-from-DHCP-podman/

Manually start daemon:

```
# /usr/libexec/cni/dhcp daemon &
```

Automatic on _Void_ linux:

```
$ sudo mkdir /etc/sv/podman_dhcp/

$ sudo cat /etc/sv/podman_dhcp/run

    exec /usr/libexec/cni/dhcp daemon

$ sudo ln -s /etc/sv/podman_dhcp /var/service/
```

Automatic on _Systemd_ linux:

```
$ cat /usr/lib/systemd/system/io.podman.dhcp.socket

    [Unit]
    Description=DHCP Client for CNI

    [Socket]
    ListenStream=%t/cni/dhcp.sock
    SocketMode=0600

    [Install]
    WantedBy=sockets.target

$ cat /usr/lib/systemd/system/io.podman.dhcp.service

    [Unit]
    Description=DHCP Client CNI Service
    Requires=io.podman.dhcp.socket
    After=io.podman.dhcp.socket

    [Service]
    Type=simple
    ExecStart=/usr/libexec/cni/dhcp daemon
    TimeoutStopSec=30
    KillMode=process

    [Install]
    WantedBy=multi-user.target
    Also=io.podman.dhcp.socket
```

Then

```
$ sudo systemctl --now enable io.podman.dhcp.socket
```

Create a container network:

```
# podman network create -d macvlan -o parent=br0 br0_net
```

Inspect the network

```
# podman network inspect br0_net

[
     {
          "name": "br0_net_2",
          "driver": "macvlan",
          "network_interface": "br0",
          "ipam_options": {
               "driver": "dhcp"
          }
     }
]
```

### Start a container

Use the created network when starting your container:

```
# podman run -it --rm --replace --name alpine --network br0_net alpine
```
