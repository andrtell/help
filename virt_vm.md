# Virtualization VMs

## Create a VM
```
$ virt-install \
  --name ubuntu01 \
  --memory 2048 \
  --vcpus 2 \
  --network network=default \
  --osinfo detect=on \
  --disk path=/some/path/ubuntu01.qcow2,size=8 \
  --cdrom /some/path/ubuntu-22.04.01-live-server-amd64.iso
```

## List VMs
```
$ virsh list --all
```

## Start a VM
```
$ virsh start ubuntu01
```

## Stop a VM gracefully (may never happen)
```
$ virsh shutdown ubuntu01
```

## Stop a VM un-gracefully
```
$ virsh destroy ubuntu01
```

## Remove a VM
```
$ virsh undefine ubuntu01 --remove-all-storage
```
