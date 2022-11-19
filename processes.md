# Processes

## List processes

List processes same effective user ID as the current user and associated with the same terminal 
as the invoker

```
$ ps
```

List every process running as current user
```
$ ps -f -u $(whoami)
```

List every process running as root
```
$ ps -f -u root
```

List every process
```
$ ps -ef
```

List with user-defined format
```
$ ps -o pid,ppid,%mem,command
```

List process tree
```
$ ps -H

$ ps --forest
````

List process tree belonging to command
```
$ ps -f --forest -C hyper
```

List pid started via command
```
$ pidof firefox

$ pgrep firefox

$ ps -C firefox
```

List children of PID
```
$ ps --ppid PARENT_PID
```

Kill process
```
$ kill 999
```

Kill process started by command
```
$ pkill firefox
```
