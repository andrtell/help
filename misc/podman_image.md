# Podman Image

`man podman-image` `man podman-pull` `man podman-push` `man podman-tag` `man podman-login`
## Images

### List images
```
$ podman iamge ls
```

### Pull an image
```
$ podman pull docker.io/library/httpd
```

### Remove an image
```
$ podman rmi docker.io/library/httpd

# or

$ podman image rm docker.io/library/httpd
```

### Build an image
```
$ podman build . -t registry.com/banana:v1
```

### Push an image
```
$ podman push registry.com/banana:v1
```

### Tag an image
```
$ podman tag docker.io/library/httpd apache
```

## Registries

### Login to a registry
```
$ podman login registry.com
```

### Registry credentials file

Podman will look for a credentials file in `~/.docker/config.json`

You can login using `podman login registry.com` then copy the file from `/run/containers/auth.json`



