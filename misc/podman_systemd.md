# Podman systemd

`man podman-generate-systemd` `man podman-auto-update`

Podman integrates well with systemd

## Generate a systemd service file that runs a container

```
$ podman generate systemd --new <container>

      [Unit]
      Description=Podman container-68a875ca8d2eb272ba83206e419e0a452d99910c8baea94f7690e3bf20711ab9.service
      Documentation=man:podman-generate-systemd(1)
      Wants=network-online.target
      After=network-online.target
      RequiresMountsFor=%t/containers

      [Service]
      Environment=PODMAN_SYSTEMD_UNIT=%n
      Restart=on-failure
      TimeoutStopSec=70
      ExecStartPre=/bin/rm \
        -f %t/%n.ctr-id
      ExecStart=/usr/bin/podman run \
        --cidfile=%t/%n.ctr-id \
        --cgroups=no-conmon \
        --rm \
        --sdnotify=conmon \
        --name the_service \
        -d \
        -p 8080:80 docker.io/library/httpd
      ExecStop=/usr/bin/podman stop \
        --ignore -t 10 \
        --cidfile=%t/%n.ctr-id
      ExecStopPost=/usr/bin/podman rm \
        -f \
        --ignore -t 10 \
        --cidfile=%t/%n.ctr-id
      Type=notify
      NotifyAccess=all

      [Install]
      WantedBy=default.target
```

## Additional parameters

You might whant to add the `--label io.containers.autoupdate=registry` flag. 

If so `podman auto-update` will update the container image.

```
ExecStart=/usr/bin/podman run \
        --cidfile=%t/%n.ctr-id \
        --cgroups=no-conmon \
        --rm \
        --sdnotify=conmon \
        -d \
        -p 8080:80 \
        --label io.containers.autoupdate=registry \
        docker.io/library/httpd
```

You may also want to add some Environment variables to the container

```
ExecStart=/usr/bin/podman run \
        --cidfile=%t/%n.ctr-id \
        --cgroups=no-conmon \
        --rm \
        --sdnotify=conmon \
        -d \
        -p 8080:80 \
        --label io.containers.autoupdate=registry \
        -e 'LOGFLARE_API_KEY=the_api_key' \
        -e 'LOGFLARE_SOURCE_ID=the_source_id \
        -e 'SECRET_KEY_BASE=the_secret_key_base' \
        docker.io/library/httpd
```

## Podman auto-update

podman auto-update looks up containers with a specified io.containers.autoupdate label (i.e., the auto-update policy).

If the label is present and set to registry, Podman reaches out to the corresponding registry to check if the image has been updated. 

An image is considered updated if the digest in the local storage is different than the one of the remote image.  

If an image must be updated, Podman pulls it down and restarts the systemd unit executing the container.

```
$ podman auto-update
```
