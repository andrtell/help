# Network Link

`man ip-link` `man ip-tuntap`

## Add bridge
```
$ ip link add bridge01 type bridge

$ ip link

  bridge01 ...
  
$ ip link set bridge01 up
```

## Add Tap device
```
$ ip tuntap add tap01 mode tap
```

## Make bridge master of tap device
```
$ ip link set tap01 master bridge01
```

## Make tap device free
```
$ ip link set tap01 nomaster
```

## Remove tap device
```
$ ip link del tap01
```

## Remove bridge
```
$ ip link del bridge01
```
