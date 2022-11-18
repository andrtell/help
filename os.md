# OS

## General information

### List OS info
```
$ uname -a
```

### List loaded kernel modules
```
$ lsmod
```

### List CPU info
```
$ lscpu
```

### List hardware info
```
$ sudo lshw -short
```

### List memory info
```
$ lsmem
```

### List block devices
```
$ lsblk
```

### List network devices
```
$ ip link
```

### List ports being listen on
```
$ sudo ss -tunlp
```

### Show disk usage
```
$ df -h
```

### Show memory usage

```
$ free -h
```

### List every process on the system
```
$ ps -ef
```

### List every process with custom info shown
```
$ ps -eo user,pid,%mem,cmd
```

### List processes with command
```
# ps -f $(pidof firefox)
```
