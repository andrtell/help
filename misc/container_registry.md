# Container registry

https://docs.docker.com/registry/deploying/

You can just follow this guide and/or use podman with systemd

## Example podman systemd service file

```
[Unit]
Description=Podman registry.service
Documentation=man:podman-generate-systemd(1)
Wants=network-online.target
After=network-online.target
RequiresMountsFor=%t/containers

[Service]
Environment=PODMAN_SYSTEMD_UNIT=%n
Restart=always
TimeoutStopSec=70
ExecStartPre=/bin/rm -f %t/%n.ctr-id
ExecStart=/usr/bin/podman run \
  --cidfile=%t/%n.ctr-id \
  --cgroups=no-conmon \
  --sdnotify=conmon \
  --rm \
  --replace \
  --name service \
  -d \
  -p 443:5000 \
  -v '/home/worker/auth/htpasswd:/auth/htpasswd:ro' \
  -v '/mnt/registry:/var/lib/registry' \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \
  -v /home/worker/cert/fullchain.pem:/cert/fullchain.pem:ro \
  -v /home/worker/cert/privkey.pem:/cert/privkey.pem:ro \
  -e "REGISTRY_HTTP_TLS_CERTIFICATE=/cert/fullchain.pem" \
  -e "REGISTRY_HTTP_TLS_KEY=/cert/privkey.pem" \
  registry:2
ExecStop=/usr/bin/podman stop --ignore --cidfile=%t/%n.ctr-id
ExecStopPost=/usr/bin/podman rm -f --ignore --cidfile=%t/%n.ctr-id
Type=notify
NotifyAccess=all

[Install]
WantedBy=default.target
```
## To generate a htpasswd file

```
podman run \
  --entrypoint htpasswd \
  httpd:2 -Bbn testuser testpassword > auth/htpasswd
```
