```bash
```bash
# General Settings
TZ=America/Chicago

# Container sources from docker hub
# Feel free to change sources of these containers
# Note bitnami/redis allows for REDIS_PASSWORD to be passed as parameter when starting container (Many others do not)
AUTHELIA_CONTAINER=authelia/authelia:latest
REDIS_CONTAINER=bitnami/redis:latest
MYSQL_CONTAINER=linuxserver/mariadb

# Ports 
AUTHELIA_PORT=8080
REDIS_PORT=6379
MYSQL_PORT=3306

# Authelia Related Settings
AUTHELIA_JWT_SECRET=<JWT_SECRET>
AUTHELIA_SESSION_SECRET=<SESSION_SECRET>

# Dou Related Settings (OPTIONAL)
#This setting is only needed if using Duo push notifications
#https://duo.com/product/multi-factor-authentication-mfa/authentication-methods/duo-push
AUTHELIA_DUO_API_SECRET_KEY=<DUO_API_SECRET_KEY>

# Redis Related Settings
AUTHELIA_SESSION_REDIS_PASSWORD=<REDIS_PASSWORD>

# MySQL/MariaDB Related Settings
AUTHELIA_STORAGE_MYSQL_DATABASE=authelia
AUTHELIA_STORAGE_MYSQL_ROOT_PASSWORD=<AUTHELIA_MYSQL_ROOT_PASSWORD>
AUTHELIA_STORAGE_MYSQL_USER=authelia
AUTHELIA_STORAGE_MYSQL_PASSWORD=<AUTHELIA_MYSQL_USER_PASSWORD>

# SMTP Related Settings
AUTHELIA_NOTIFIER_SMTP_PASSWORD=<SMTP_PASSWORD>

## Volume Mounts
AUTHELIA_CONFIG_VOLUME=/etc/authelia/config/configuration.yml
MYSQL_DATA_VOLUME=/var/data/db
# Following Volumes only needed if using a single file rather than LDAP for users
AUTHELIA_USERS_VOLUME=/etc/authelia/config/users.yml
AUTHELIA_USER_DB_VOLUME=/var/data/authelia/db.sqlite3

```
```
