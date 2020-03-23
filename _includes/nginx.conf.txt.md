```bash
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
	                    '$status $bytes_sent "$http_referer"'
                      '"$http_user_agent" "$http_x_forwarded_for"';

    types_hash_max_size 4096;

    # Redirect all HTTP traffic to HTTPS
    server {

        listen 80 default_server;
        listen [::]:80 default_server;

        return 301 https://$host$request_uri;
    }

    # Import server blocks for all subdomains
    include "vdomains/*.conf";
}

```
