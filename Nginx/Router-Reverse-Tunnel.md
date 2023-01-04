# A Guide for setting up a reverse proxy with some routers
#### Note: Some routers (such as BT home hubs) will block access / present 403 forbidden if the request is coming from outside of the routers internal address range to get around this we can spoof the host header to that of the routers ip

On your router or another machine on your network
```
ssh -vnNT -R 8040:localhost:80 user@hostname.co.uk
```

Nginx config
```
map $http_upgrade $connection_upgrade {
default upgrade;
'' close;
}

server {
  listen 443;
  server_name router.hostname.co.uk;
  location / {
        proxy_hide_header Access-Control-Allow-Origin;
        add_header Access-Control-Allow-Origin "*" always;
        proxy_pass https://127.0.0.1:8040/;
        proxy_set_header Host 192.168.1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }

}
```

After reloading nginx you should be presented with your router web ui at router.hostname.co.uk
