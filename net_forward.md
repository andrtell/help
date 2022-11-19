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
$ nft -f <(cat <<_EOF
table filter {
  chain input { ty filter hook input priority 0; policy accept; 
  chain forward { ype filter hook forward priority 0; policy accept;}
  chain output { ype filter hook output priority 0; policy accept;}
}

table nat {
  chain prerouting {type nat hook prerouting priority 0; policy accept;}
  chain postrouting {type nat hook postrouting priority 0; policy accept;}
  chain output {type nat hook output priority 0; policy accept;}
}
_EOF
)
```

### Add SNAT forwarding using nftables
```
$ nft add rule ip nat postrouting oif eth0 masquerade
```

### Add DNAT forwading using nftables
```
$  nft add rule ip nat prerouting iif eth0 tcp dport 9999 dnat to 10.0.0.7:9999
```
