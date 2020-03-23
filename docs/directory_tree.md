---
layout: page
title: Directory Tree
permalink: /directory_tree/
nav_order: 9
---

# Directory Tree[^1],[^2]

```bash
authelia
|-- etc
|   |-- authelia
|   |   `-- config
|   |       |-- configuration.yml
|   |       `-- users.yml
|   |-- docker
|   |   `-- compose
|   |       `-- authelia
|   |           |-- docker-compose.yml
|   |           `-- .env
|   |-- nginx
|   |   |-- nginx.conf
|   |   |-- snippets
|   |   |   |-- authelia.location.conf
|   |   |   |-- authelia.domain.com.cert.conf
|   |   |   |-- authelia_auth.conf
|   |   |   |-- authelia_proxy.conf
|   |   |   |-- ssl-modern-params.conf
|   |   |   |-- ssl-params.conf
|   |   |   |-- backend.domain.com.cert.conf
|   |   |   `-- internal-access-rules.conf
|   |   `-- vdomains
|   |       `-- backend.domain.com.conf
|   |-- systemd
|   |   `-- system
|   |       |-- docker-compose-reload@.service
|   |       |-- docker-compose-reload@.timer
|   |       `-- docker-compose@.service
|   `-- letsencrypt
|       |-- authelia.<domain.com>
|       |   |-- fullchain.pem
|       |   |-- priv.key
|       |   |-- chain.pem
|       |   `-- cert.pem
|       `-- <domain.com>-"Protected domain"
|           |-- fullchain.pem
|           |-- priv.key
|           |-- chain.pem
|           `-- cert.pem
`-- var
    `-- data
        |-- authelia
        |   `-- db.sqlite3
        `-- db

```

Courtesy: [^1]


Directory structure can be created via the following commands (Please run the commands with **sudo** or as root user):
```bash
mkdir -p /etc/authelia/config
mkdir -p /etc/docker/compose/authelia
mkdir -p /etc/nginx/snippets /etc/nginx/vdomains
mkdir -p /var/data/authelia /var/data/db
```
[^1]:[JS Fiddle]({{ site.data.links.js_fiddle}})
[^2]: [Reference Files]()
