# Podman Run

## Run container

```
$ podman run registry.access.redhat.com/ubi8 bash -c "sleep 10"
```

## Run container wth interactive terminal
```
$ podman run -it --rm registry.access.redhat.com/ubi8 bash
```

### Run container as daemon
```
$ podman run -d -p 8080:80 --name httpd --rm docker.io/library/httpd:2.4
```

## List containers

```
$ podman ps --all
```

## Stop container

```
$ podman stop CONTAINER
```

## Remove a container
```
$ podman rm CONTAINER
```
