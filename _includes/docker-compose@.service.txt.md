```bash
[Unit]
Description=Docker Compose %i container starter
After=docker.service network-online.target
Requires=docker.service network-online.target

[Service]
WorkingDirectory=/etc/docker/compose/%i
Type=oneshot
RemainAfterExit=yes

ExecStartPre=/usr/bin/docker system prune
ExecStartPre=/usr/bin/docker-compose down -v
ExecStartPre=/usr/bin/docker-compose rm -fsv
ExecStartPre=/usr/bin/docker-compose pull --quiet
#ExecStartPre=/usr/bin/docker-compose build --compress
ExecStart=/usr/bin/docker-compose up -d

ExecStop=/usr/bin/docker-compose down -v
ExecStopPost=/usr/bin/docker-compose rm -fv

ExecReload=/usr/bin/docker-compose pull --quiet
ExecReload=/usr/bin/docker-compose up -d

[Install]
WantedBy=multi-user.target
```
