```bash
###############################################################
#                         Users Database                      #
###############################################################
# This file can be used if you do not have an LDAP set up. All users passwords are 'password'.
users:
  john:
    password: "$6$rounds=500000$jgiCMRyGXzoqpxS3$w2pJeZnnH8bwW3zzvoMWtTRfQYsHbWbD/hquuQ5vUeIyl9gdwBIt6RWk2S6afBA0DPakbeWgD/4SZPiS0hYtU/"
    email: john.doe@authelia.com
    groups:
      - admins
      - dev
  harry:
    password: "$6$rounds=500000$jgiCMRyGXzoqpxS3$w2pJeZnnH8bwW3zzvoMWtTRfQYsHbWbD/hquuQ5vUeIyl9gdwBIt6RWk2S6afBA0DPakbeWgD/4SZPiS0hYtU/"
    email: harry.potter@authelia.com
    groups: []
  bob:
    password: "$6$rounds=500000$jgiCMRyGXzoqpxS3$w2pJeZnnH8bwW3zzvoMWtTRfQYsHbWbD/hquuQ5vUeIyl9gdwBIt6RWk2S6afBA0DPakbeWgD/4SZPiS0hYtU/"
    email: bob.dylan@authelia.com
    groups:
      - dev
  james:
    password: "$6$rounds=500000$jgiCMRyGXzoqpxS3$w2pJeZnnH8bwW3zzvoMWtTRfQYsHbWbD/hquuQ5vUeIyl9gdwBIt6RWk2S6afBA0DPakbeWgD/4SZPiS0hYtU/"
    email: james.dean@authelia.com
```
