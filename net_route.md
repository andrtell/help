# Net Route

see: `man ip-route`

## Show routes
```
$ ip route
```

## Add a route
```
$ ip route add 192.168.10.10/24
```

## Add a default route (i.e gateway)
```
$ ip route add default via 10.0.0.7 dev eth0
```
