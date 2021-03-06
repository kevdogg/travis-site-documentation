```bash
###############################################################
#                Authelia minimal configuration               #
###############################################################

#Please modify port number for your needs - Make sure port number is 
#reflected in nginx config files
port: 8080

# logs_level parameter is optional.  Values = trace,debug,info
logs_level: trace
authentication_backend:
  file:
    path: /etc/authelia/users.yml
totp:
  issuer: authelia.<domain.com>

duo_api:
  hostname: <duo_hostname>
  integration_key: <duo_integration_key>

session:
  # The name of the session cookie. (default: authelia_session).
  name: authelia_session_id

  # The secret to encrypt the session cookie.
  # This secret can also be set using the env variables AUTHELIA_SESSION_SECRET
  
  # The domain to protect.
  # Note: the login portal must also be a subdomain of that domain.
  domain: <domain.com>
  
  # The time in seconds before the cookie expires and session is reset.
  expiration: 3600 # 1 hour

  # The inactivity time in seconds before the session is reset.
  inactivity: 300 # 5 minutes
  
  redis:
    host: redis
    port: 6379

storage:
  mysql:
    host: mariadb
    port: 3306
    database: authelia
    username: authelia

#  local:
#    path: /var/lib/authelia/db.sqlite3

access_control:
  default_policy: deny
  rules:

    - domain: authelia.<domain.com>
      policy: bypass

    - domain: <domain.com>
      policy: two_factor

#    - domain: "*.<domain.com>"
#      subject: "group:admin"
#      policy: two_factor

regulation:
  # The number of failed login attempts before user is banned.
  # Set it to 0 to disable regulation.
  max_retries: 3

  # The time range during which the user can attempt login before being banned.
  # The user is banned if the authentication failed `max_retries` times in a `find_time` seconds window.
  find_time: 120

  # The length of time before a banned user can sign in again.
  ban_time: 300

notifier:
# : filesystem:
#    filename: /var/data/authelia/emails.txt
  smtp:
    username: smtp
    host: postfix.<domain.com>
    port: 587
    sender: postmaster@authelia.<domain.com>
```
