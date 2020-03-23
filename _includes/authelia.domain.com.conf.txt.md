```bash
server {
    listen         80;
    server_name    authelia.<domain.com>;
    return         301 https://$server_name$request_uri;
}

server {
    listen [::]:443 ssl http2;
    listen      443 ssl http2;
    server_name authelia.<domain.com>;

    access_log /var/log/nginx/authelia.<domain.com>.access.log main buffer=32k;
    error_log /var/log/nginx/authelia.<domain.com>.error.log warn;

    include snippets/authelia.<domain.com>.cert.conf;
    include snippets/ssl-params.conf;

    location / {
        
        # If ngnix is run within a docker container, the following line would be
        # set $upstream_authelia http://authelia:8080.-> Please adjust port number to
        # port number which is specified for the authelia service within docker-config.yml

        set $upstream_authelia http://authelia.<domain.com>:8080; 
        proxy_pass $upstream_authelia;
        include snippets/authelia_proxy.conf;  
    }
} 
```
