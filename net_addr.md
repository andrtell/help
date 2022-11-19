# Network address

## Manage IP addresses

### Show interface address information
```
# ip address list
```

### Add IP address
```
$ ip address add 10.0.0.9/24 dev enp1s0 
```

### Remove IP address
```
$ ip address del 10.0.0.9/24 dev enpis0
```

### Show routes
```
$ ip route
```

### Add a route
```
$ ip route add 192.168.10.10/24
```

### Add a default route (i.e gateway)
```
$ ip route add default via 10.0.0.7 dev eth0
```
