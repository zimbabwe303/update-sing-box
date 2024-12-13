# update-sing-box

A very simple script for auto-updating sing-box from the git sources right on the server when you are connected to it via itself, i.e. via the same proxy server (`ssh -p <port> -o ProxyCommand='nc -X 5 -x 127.0.0.1:<proxy_port> %h %p' <user>@<ip_address>`)

Basically what it does: `git pull`, `make`, `stop service`, `cp` and `restart service`. Also does preliminary checks for the system requirements.

What it does not: check if the updated sing-box actually starts, so if it fails then you have to login via another proxy server or through a direct connection.

Edit the variables at the beginning of the script to your requirements.

## systemd service

The updater is intended to be used with sing-box as a systemd service.

Here is a template for the `sing-box.service` file:

```
[Unit]
Description=sing-box Service
Documentation=https://sing-box.sagernet.org
After=network.target nss-lookup.target

[Service]
DynamicUser=yes
WorkingDirectory=/var/lib/sing-box
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
NoNewPrivileges=true
ExecStart=/usr/local/bin/sing-box run -c /var/lib/sing-box/config.json
ExecReload=/bin/kill -HUP \$MAINPID
Restart=on-failure
RestartSec=10
LimitNPROC=10000
LimitNOFILE=1000000

[Install]
WantedBy=multi-user.target
```

Installation tips:

1. Create a file `/etc/systemd/system/sing-box.service` and paste the contents above there.
2. Create the directory `/var/lib/sing-box` and place your `config.json` there.
3. Don't forget to do `systemctl enable sing-box.service` before trying `systemctl start sing-box.service`.
