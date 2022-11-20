# Podman Container

see:
`man podman`
`man podman-run`
`man podman-stop`
`man podman-rm`
`man podman-kill`
`man podman-exec`
`man podman-attach`


## List containers
```
$ podman ps --all
```

## Stop container
```
$ podman stop <container>
```

## Remove a container
```
$ podman rm <container>
```

## Run container
```
$ podman run registry.access.redhat.com/ubi8 bash -c "sleep 10"
```

## Run container wth interactive terminal
```
$ podman run -it --rm registry.access.redhat.com/ubi8 bash
```

## Run container as daemon
```
$ podman run -d -p 8080:80 --name httpd --rm docker.io/library/httpd:2.4
```

## Kill the main process in a container
```
$ podman kill <container>
```

## Detach from container (while it is running interactive or printing output)
```
$ podman run -it --name orange registry.access.redhat.com/ubi
  
    # <c-p><c-q>
```

## Attach to container (to continue interactive terminal or view output)
```
& podman attach orange
```

## Execute command in running container
```
$ podman run --rm -it --name orange registry.access.redhat.com/ubi8 bash -c "sleep 1000"
  
      # <c-p><c-q>
      
$ podman exec -it orange bash
```
