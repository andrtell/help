# Network Forward

## Setup

### Show if IP forwarding is enabled on host

```
$ cat /proc/sys/net/ipv4/ip_forward

# or

$ sysctl net.ipv4.ip_forward
```

### Enable IPv4 forwarding of IP packets on host

```
$ echo 1 > /proc/sys/net/ipv4/ip_forward

# or

$ sysctl -w net.ipv4.ip_forward=1
```

### To persist

```
$ echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
```

### Setup default rules for nftables

Note that the default policies here are ACCEPT.

```
$ nft flush ruleset # remove all rules

$ nft -f <(cat <<_EOF
table filter {
  chain input { type filter hook input priority 0; policy accept; }
  chain forward { type filter hook forward priority 0; policy accept; }
  chain output { type filter hook output priority 0; policy accept; }
}

table nat {
  chain prerouting { type nat hook prerouting priority 0; policy accept; }
  chain postrouting { type nat hook postrouting priority 0; policy accept; }
  chain output { type nat hook output priority 0; policy accept; }
}
_EOF
)
```

### Add SNAT forwarding using nftables

This will enable any host on network that has THIS host setup as a gateway (default route) to 
establish a connection to the outsite world.

In other words setup host as a FORWARD proxy (level 3).

```
$ nft add rule ip nat postrouting oif eth0 masquerade
```

### Add DNAT forwading using nftables

This will forward any traffing comming in on port 9999 on eth0 to 10.0.0.7:9999

In other words setup host as a REVERSE proxy (level 3).

```
$  nft add rule ip nat prerouting iif eth0 tcp dport 9999 dnat to 10.0.0.7:9999
```
