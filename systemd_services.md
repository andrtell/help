# Systemd services

## Enable persistent user-services

Services started by non-root user dies after user logs out unless:

```
$ loginctl enable-linger <username>
```

## Whenever changes has been made to a service file:
```
$ systemctl daemon-reload
```

## List services
```
$ systemctl list-units --type service
```

## Stop service
```
$ systemctl stop <service>
```

## Start service
```
$ systemctl start <service>
```

## Reload service

Note, this will reload the service specific config, NOT the unit file (on disk). Use `systemctl daemon-reload` for this.

```
$ systemctl reload <service>
```

## Persist service (restarts on reboot)

```
$ systemctl enable <service>
```

## Show service status
```
$ systemctl status <service>
```

## Creating service (unit) files

root services goes into `/etc/systemd/system/<name>.service`

user services goes into `/.config/systemd/user/<name>.service`

### Format

```
[Unit]
Descripttion=My service

[Service]
ExecStart=/home/user/my-script.sh

[Install]
WantedBy=multi-user.target
```
