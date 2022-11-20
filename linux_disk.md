# Linux Disk

`man mount` `man lsblk` `man mkfs`

## Mount & Umount

```
$ lsblk

    sdb                 8:16   1  14.4G  0 disk  
    ├─sdb1              8:17   1   2.4G  0 part
    ├─sdb2              8:18   1     4M  0 part  
    └─sdb3              8:19   1    12G  0 part  

$ mount /dev/sdb1 /mnt/sd

$ lsblk

    sdb                 8:16   1  14.4G  0 disk  
    ├─sdb1              8:17   1   2.4G  0 part  /mnt/sd
    ├─sdb2              8:18   1     4M  0 part  
    └─sdb3              8:19   1    12G  0 part  

$ umount /mnt/sd

$ lsblk

    sdb                 8:16   1  14.4G  0 disk
    ├─sdb1              8:17   1   2.4G  0 part
    ├─sdb2              8:18   1     4M  0 part  
    └─sdb3              8:19   1    12G  0 part  
```

## Remove paritions from disk

```
$ lsblk

    sdb                 8:16   1  14.4G  0 disk  
    ├─sdb1              8:17   1   2.4G  0 part
    ├─sdb2              8:18   1     4M  0 part  
    └─sdb3              8:19   1    12G  0 part  

$ fdisk /dev/sdb

    Command (m for help): p
  
      Disk /dev/sdb: 14.41 GiB, 15472047104 bytes, 30218842 

      Device     Boot   Start      End  Sectors  Size Id Type
      /dev/sdb1  *          0  5013503  5013504  2.4G  0 Empty
      /dev/sdb2           324     8515     8192    4M ef EFI (FAT-12/16/32)
      /dev/sdb3       5013504 30218841 25205338   12G 83 Linux
      
    Command (m for help): d
      
      Partition had been deleted
      
    Command (m for help): w
```

## Add partition to disk

```
$ lsblk

    sdb                 8:16   1  14.4G  0 disk  

$ fdisk /dev/sdb

    Command (m for help): n
    
    Command (m for help): w
    
$ lsblk

    sdb                 8:16   1  14.4G  0 disk  
    └─sdb1              8:17   1  14.4G  0 part  
```

## Make filesystem

```
$ lsblk -f
    
    sdb                                                                            
    └─sdb1
    
$ mkfs -t ext4 /dev/sdb1

$ lsblk -f

    sdb                                                                            
    └─sdb1
          ext4   1.0         7387ef27-3911-48dd-b490-079139e75ae6   
```
