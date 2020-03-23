---
title: nginx
tags: ["nginx", "authelia"]
permalink: /nginx/
nav_order: 30

ngnix_conf_includes: nginx.conf.txt.md
authelia_conf_includes: authelia.domain.com.conf.txt.md
---
{% comment %}
<div class="outerbox">
 <div class="middlebox">
  <div class="box">
    <div> <img src="/assets/images/nginxlogo2.png" alt="nginxlogo"></div>
    <div><a href="{{ site.data.links.nginx }}">Nginx</a></div>
  </div>
 </div>
 <div class="middlebox">
  <div class="box">
   <div class="growtall"> <img src="/assets/images/authelialogo.png" alt="Authelia"></div>
   <div><a href="{{ site.data.links.authelia }}">Authelia</a></div>
  </div>
 </div>
</div>
{% endcomment %}
{% comment %}
<div class="wrapper">
 <div class="enclosed1">
    <div> <img class="middle" src="/assets/images/nginxlogo3.png" alt="nginxlogo"></div>
    <div><a href="{{ site.data.links.nginx }}">Nginx</a></div>
 </div>
<div class="enclosed2">
    <div> <img class="middle" src="/assets/images/nginxlogo3.png" alt="nginxlogo"></div>
    <div><a href="{{ site.data.links.nginx }}">Nginx</a></div>
</div>
</div>
{% endcomment %}

<div class="imagerow">
  <div class="imagecol">
    <div class="growtall"><img src="/assets/images/nginxlogo2.png" alt="nginxlogo"></div>
    <div><a href="{{ site.data.links.nginx }}">Nginx</a></div>
  </div>
  <div class="imagecol">
    <div class="growtall"><img src="/assets/images/authelialogo.png" alt="Authelia"></div>
    <div><a href="{{ site.data.links.authelia }}">Authelia</a></div>
  </div>
</div>

# Nginx Setup with Authelia
[Samuel Dowling's Nginx Reverse Proxy Guide]({{ site.data.links.samuel_dowling_nginx_reverse_proxy}})[^1] is a very good resource on how to setup and configure nginx as a reverse proxy. Based on his recommendation, I'm using his structure as a guide how to configure nginx for this guide.

The basic layout for nginx for this guide is the following:

```bash
/etc/nginx
|-- vdomains
|   |-- authelia.<domain.com>
|   `-- <domain.com>
|-- snippets
|   |-- authelia.location.conf
|   |-- authelia.<domain.com>.cert.conf
|   |-- authelia.<domain.com>.conf
|   |-- authelia_auth.conf
|   |-- authelia_proxy.conf
|   |-- ssl-modern-params.conf
|   |-- ssl-params.conf
|   |-- <domain.com>.cert.conf
|   |-- <domain.com>.conf
|   |-- proxy-ssl.params.conf
|   `-- internal-access-rules.conf
`-- nginx.conf
```

At least two virtual domains need to be configured:
<ul>
  <li><strong>authelia.&lt;domain.com&gt;</strong></li>
  <li><strong>&lt;domain.com&gt;</strong></li>
</ul>

**authelia.\<domain.com\>** = the authelia registration and authentication portal

**\<domain.com\>** = the domain/web application that utilizes authelia as an authentication frontend..
  - Multiple domains/sites can utilize authelia for frontend authentication depending on the reverse proxy configuration. 
  - Examples of type of locations which utilize authelia as a frontend
    - Top Level Domains: domain.com, yourdomain.org, happiness.biz
    - SubDomains: subdomain1.domain.com, whereis.yourdomain.org
    - Folders: domain.com/admin, yourdomain.org/lost/img 

# Example Configuration
  Please adapt to your setup, particularly if you have a pre-existing configured nginx reverse proxy.

  My main /etc/nginx/nginx.conf file is very basic. All the virtual domains are included within the /etc/nginx/vdomains folder and includes are located within the /etc/nginx/includes folder.

## nginx.conf
{% capture text %}{% include {{ page.ngnix_conf_includes }} %}{% endcapture %}
{{ text | markdownify }}

## authelia.\<domain.com\> virtual domain file
   Located at: /etc/nginx/vdomains/authelia.\<domain.conf\>

## Example Domain which utilizes authelia as an authentication frontend - \<domain.com\>

The full example file is found here: [authelia.domain.com.conf]({{ site.data.links.authelia.domain.com.conf }})

Below are two types locations that authelia could provide authentication services for:
  1. Top Level Domain
  2. Subfolder of the Top Level Domain

```bash
...
...
# Top Level Domain Example

       location /{
          #auth_basic "Private";
          #auth_basic_user_file /etc/nginx/private/admin.scr;

           include snippets/proxy-ssl-params.conf;
           #include snippets/internal-access-rules.conf;	

           set $upstream_bw https://<domain.com>:443;
           proxy_pass $upstream_bw;
           include snippets/authelia_auth.conf;
           include snippets/authelia_proxy.conf;
           #proxy_redirect off;
       }
```

```bash
...
...
# Protected Subfolder Location of the Top Level Domain

       location /admin {
		       #auth_basic "Private";
		       #auth_basic_user_file /etc/nginx/private/admin.scr;
		
		       include snippets/proxy-ssl-params.conf;
           #include snippets/internal-access-rules.conf;	

		       set $upstream_bw https://<domain.com>:443;
    		   proxy_pass $upstream_bw;
		       include snippets/authelia_auth.conf;
		       include snippets/authelia_proxy.conf;
		       #proxy_redirect off;
	      }
...
...
```

In setup above, the protected domain/subfolder is https://\<domain.com\>/admin.
No auth_basic, or auth_basic_user_files are needed since authelia is going to be performing the authentication for the site.
proxy-ssl-params.conf - Includes parameters since this reverse proxy is going to be re-encrypting the connection to the backend. These parameters will control the re-encryption.  This file is not needed if the reverse proxy is terminating the SSL encryption and passing unencrypted to the backend
internal-access-rules.conf - Include if wanting to control access from the site from various IP addressess
$upstream-bw - This variable sets the location of the backend encrypted domain. An http or https transport layer may be utilized depending if there are SSL certificates for the backend.  With no certificates this variable would be: http://backend.\<domain.com\>:80
authelia_auth.conf, authelia_proxy.conf - Parameters needed by the authelia authentication portal

{% capture text %}{% include {{ page.authelia_conf_includes }} %}{% endcapture %}
{{ text | markdownify }}


```bash
{* include nginx.conf.txt *}
```

### Page Links
[^1]: [Samuel Dowling's Nginx Reverse Proxy Guide]({{ site.data.links.samuel_dowling_nginx_reverse_proxy}})

