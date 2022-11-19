# Certificates

## Create certificate using CertBot

Before you start, make sure the host is reachable through its domain (i.e host.com) by setting up your DNS.

Install and run certbot

```
# update snap
$ sudo snap install core; sudo snap refresh core

# install certbot
$ sudo snap install --classic certbot

# make it runable
$ sudo ln -s /snap/bin/certbot /usr/bin/certbot

# run it
$ sudo certbot certonly --standalone -d host.com -d '*.host.com'
```

The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically 
before they expire. 

You will not need to run Certbot again, unless you change your configuration. 

Certificate files can be found at:

```
/etc/letsencrypt/live/<domain>/fullchain.pem;
/etc/letsencrypt/live/<domain>/privkey.pem;
```    

### Note

If you are behind a load-balancer, you should probably run this on the load-balancer host and not on 
a host behind the load-balancer.

# Create certificates manually

This example is using a digital ocean (digital ocean allows you to set DNS entries via api)

Replace host.com with your domain.

```
export TMP_DIR=/tmp/certbot/volumes
export EXPECTED_KEYS_PATH=$TMP_DIR/etc/letsencrypt/live/host.com/

$ mkdir -p $TMP_DIR/etc/letsencrypt
$ mkdir -p $TMP_DIR/var/lib/letsencrypt

$ podman  run -it --rm --name certbot \
          -v "/tmp/certbot/volumes/etc/letsencrypt:/etc/letsencrypt" \
          -v "/tmp/certbot/volumes/var/lib/letsencrypt:/var/lib/letsencrypt" \
          -v "$CREDENTIALS_FILE:/do.ini" \
          certbot/dns-digitalocean certonly \
                    --dns-digitalocean \
                    --dns-digitalocean-credentials /do.ini \
                    -m 'hostmaster@host.com' \
                    --agree-tos \
                    -n \
                    -d host.com \
                    -d '*.host.com'

 cp $EXPECTED_KEYS_PATH/fullchain.pem .
 cp $EXPECTED_KEYS_PATH/privkey.pem .
