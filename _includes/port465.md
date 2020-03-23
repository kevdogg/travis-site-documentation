```bash
## Enable Options for Either Port 587 or Port 465

## TLS Option (Port 587)
#smtp_tls_security_level = encrypt
#Gmail SMTP
#relayhost = [smtp.gmail.com]:587
#smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd_GMAIL_587
## End TLS Options (Port 587)

## TLS Option (Port 465)
smtp_tls_security_level = encrypt
smtp_tls_wrappermode = yes
#Gmail SMTP
relayhost = [smtp.gmail.com]:465
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd_GMAIL_465
## End TLS Option (Prot 465)
```
