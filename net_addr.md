# Network address

see: `man ip-address`

## Show interface address information
```
# ip address list
```

## Add IP address
```
$ ip address add 10.0.0.9/24 dev enp1s0 
```

## Remove IP address
```
$ ip address del 10.0.0.9/24 dev enpis0
```
