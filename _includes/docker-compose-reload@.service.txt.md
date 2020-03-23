```bash
[Unit]
Description=Refresh images and update containers

[Service]
Type=oneshot

ExecStart=/usr/bin/systemctl reload-or-restart docker-compose@%i.service

```
