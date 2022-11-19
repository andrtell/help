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
````

List pid started via command
```
$ pidof firefox

$ ps $(pidof firefox)

$ pgrep firefox
```

Kill process
```
$ kill 999
```

Kill process started by command
```
$ pkill firefox
```
