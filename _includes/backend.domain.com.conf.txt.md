```bash
server {
        listen [::]:443 ssl http2;
	listen      443 ssl http2;

        server_name bw.<domain.com>;

        #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
        #                  '$status $body_bytes_sent "$http_referer" '
        #                  '"$http_user_agent" "$http_x_forwarded_for"';

        access_log /var/log/nginx/bw.<domain.com>.access.log main buffer=32k;
        error_log /var/log/nginx/bw.gohilton.error.log;

        include snippets/bw.<domain.com>.cert.conf;
        #include snippets/ssl-params.conf;
	include snippets/ssl-modern-params.conf;

	#include snippets/authelia_check-auth_block_internal_api.conf;

	keepalive_timeout 65;

	# Allow large attachments
	client_max_body_size 128M;
	
	include snippets/authelia.location.conf;

	location / {
		#proxy_pass http://backend.<domain.com>:80;
		
		include snippets/proxy-ssl-params.conf;
		include snippets/proxy-params.conf;
                include snippets/internal-access-rules.conf;	

		#include snippets/authelia_default_headers.conf;
		#include snippets/authelia_sso_params.conf;
		#include snippets/authelia_proxy_params.conf;

		proxy_pass https://backend.<domain.com>;
	}

	location /notifications/hub {
    		proxy_pass http://backend.<domain.com>:3012;
    		proxy_set_header Upgrade $http_upgrade;
    		proxy_set_header Connection "upgrade";
  	}

	location /notifications/hub/negotiate {
    		proxy_pass http://backend.<domain.com>:80;
  	}	

        location /admin {
		#https://github.com/dani-garcia/bitwarden_rs/wiki/Proxy-examples
		#auth_basic "Private";
		#auth_basic_user_file /etc/nginx/private/admin.scr;
		
		include snippets/proxy-ssl-params.conf;
                #include snippets/internal-access-rules.conf;	
		#include snippets/proxy-params.conf;

		#include snippets/authelia_default_headers.conf;
		#include snippets/authelia_sso_params.conf;
		#include snippets/authelia_proxy_params.conf;

		set $upstream_bw https://backend.<domain.com>:443;
    		proxy_pass $upstream_bw;
		include snippets/authelia_auth.conf;
		include snippets/authelia_proxy.conf;
		#proxy_redirect off;
	}

	#location / {
	#	root   /usr/share/nginx/html;
	#	index  index.html index.htm;
        #        #include snippets/proxy-params.conf;
        #        #include snippets/internal-access-rules.conf;
        #        #proxy_pass http://192.168.0.12;
        #}
}

```
