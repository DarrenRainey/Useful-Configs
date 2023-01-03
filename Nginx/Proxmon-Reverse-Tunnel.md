# A Guide for setting up a reverse proxy with NGINX on a remote host for proxmox
#### Note: Access-Control header is required for shell access as without it Content Security Policy (CSP) will block websockets.

On your proxmox machine or another machine on your network
```
ssh -vnNT -R 8006:localhost:8006 user@hostname.co.uk
```

Nginx config
```
map $http_upgrade $connection_upgrade {
default upgrade;
'' close;
}

server {
  listen 443;
  server_name proxmox.hostname.co.uk;
  location / {
        proxy_hide_header Access-Control-Allow-Origin;
        add_header Access-Control-Allow-Origin "*" always;
        proxy_pass https://127.0.0.1:8006/;
        proxy_set_header Host $host;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }

}
```

After reloading nginx you should be presented with your proxmox web ui at proxmox.hostname.co.uk
