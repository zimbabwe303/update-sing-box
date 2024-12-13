# update-sing-box

A very simple script for auto-updating sing-box from the git sources right on the server when you are connected to it via itself, i.e. via the same proxy server (`ssh -p <port> -o ProxyCommand='nc -X 5 -x 127.0.0.1:<proxy_port> %h %p' <user>@<ip_address>`)

Basically what it does: `git pull`, `make`, `stop service`, `cp` and `restart service`. Also does preliminary checks for the system requirements.

What it does not: check if the updated sing-box actually starts, so if it fails then you have to login via another proxy server or through a direct connection.

Edit the variables at the beginning of the script to your requirements.