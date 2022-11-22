# Manage VMs

## Create VMs

Do create a new VM from scratch

```
$ virt-install \
  --name ubuntu01 \
  --memory 2048 \
  --vcpus 2 \
  # --network network=default \
  --bridge br0 \
  --osinfo detect=on \
  --disk path=/some/path/ubuntu01.qcow2,size=8 \
  --console pty,target_type=serial \
  --cdrom /some/path/ubuntu-22.04.01-live-server-amd64.iso
```

Once the VM is created you can clone it

```
$ virt-clone -o ubuntu01 -n ubuntu02 -f /some/path/ubuntu02.qcow2
```

## Manage VMs

You can list all your VMs using

```
$ virsh list --all
```

You can start a VM 
```
$ virsh start ubuntu01
```

You can shutdown/destroy a VM
```
$ virsh shutdown ubuntu01
$ virsh destroy ubuntu01
```

You can remove a VM
```
$ virsh undefine ubuntu01 --remove-all-storage
```
