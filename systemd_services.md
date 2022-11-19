# Systemd services

## Enable persistent user-services

Services started by non-root user dies after user logs out unless:

```
$ loginctl enable-linger <username>
```
