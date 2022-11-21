# Jobs

From `man bash`

Job control refers to the ability to selectively stop (suspend) the
execution of processes and continue (resume) their execution at a later
point.  

The shell associates a job with each pipeline.  

When bash starts a job asynchronously (in the background), it prints a
line that looks like:

``` 
$ sleep 3000 &
     
    [1] 25647
 ```
 
indicating that this job is job number 1 and that the process ID of the
last process in the pipeline associated with this job is 25647.  

The operating system maintains the notion of a current terminal process
group ID.  

```
$ sleep 4000 &

  [1] 23945
```

Members of this process group (processes whose process group ID is equal 
to the current terminal process group ID) receive keyboard-generated signals such as SIGINT.

These processes are said to be in the foreground.  

Background processes are those whose process group ID differs from the terminal's;

Job number n may be referred to as %n.  

```
$ jobs %1
$ kill %1
```

A job may also be referred to using a prefix of the name used to start it.

```
$ echo | sleep 1000 &
$ jobs %echo
```

Using %?ce, on the other hand, refers to any job _containing_ the string ce in its command line. 

```
$ jobs %?sleep
```

The symbols %% and %+ refer to the shell's notion of the current job, which is the last job stopped
while it was in the foreground or started in the background.  

```
$ jobs %+
```

Thep revious job may be referenced using %-. 

```
$ jobs %-
```

Simply naming a job can be used to bring it into the foreground: `%1` is a synonym for `fg %1`

```
$ sleep 3000 &
$ fg %1
```
