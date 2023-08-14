# Install and config the Nginx

* Install `Nginx` service to the Server

```bash
sudo apt update
sudo apt install nginx
```

* Add a configuration for your Domain name Assume your domain name is `*.example.com`

```
vi /etc/nginx/conf.d/example.conf
```

```bash
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
        listen 80;
        listen [::]:80;
        server_name *.example.com;                                           # need to your domain
        return 301 https://$host$request_uri;
        #client_max_body_size 1G;
}
server {
        listen 443 ssl;
        listen [::]:443 ssl;
        ssl_certificate  /etc/letsencrypt/live/example.com/fullchain.pem;     # need to config SSL certificate
        ssl_certificate_key  /etc/letsencrypt/live/example.com/privkey.pem;   # need to config SSL certificate

        server_name *.example.com;                                            # need to config your domain
        location / {
          proxy_pass http://127.0.0.1:<port>;  	# Need to configure the Intranet port corresponding to ingress-nginx-controller service port 80 
          proxy_set_header Host $http_host;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection $connection_upgrade;
       }
}
```

* **Note:**
  * `server_name`: a generic domain name
  * `ssl_certificate` and `ssl_certificate_key`: certificate for https.
  * `proxy_pass`: The port should be the Intranet port corresponding to `ingress-nginx-controller` service port 80
*   Reload the `Nginx` config

    ```bash
    sudo nginx -s reload
    ```
* Map your "catch-all (wildcard) subdomain(\*.example.com)" to a public IP address
