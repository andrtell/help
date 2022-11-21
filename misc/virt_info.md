# Virtualization Info

`man virsh`

## Check what can be assigned to a VM
```
$ virsh domcapablities

$ virsh domcapabilites | grep diskDevice -A 7
```

## Number of vCPUs

If running task with a high cpu workload use 1 vCPU per 1 pCPU. In any case, the KVM hypervisor supports overcommitting virtualized CPUs (vCPUs). 

To check the max number of vCPUs that can be assign to a VM
```
$ virsh maxvcpus

# or

$ virsh domcapabilities | grep max
```
