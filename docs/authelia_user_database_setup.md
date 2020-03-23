---
layout: default 
title: Authelia User Database Setup - users.yml
tags: ["database", "authelia"]
permalink: /authelia_user_database_setup/
nav_order: 130
---

![Authelia logo](/assets/images/authelia-title.png)

## Authelia User Database Backend

The Authelia User Database is an authentication backend used for verifying user passwords and used to retrieve information such as email address and groups to which a user belongs. This database works in a harmony Authelia's Access Control List (ACL) to control site access.  ACL setup is discussed [here]().

The Authelia User Database contains entries specifc for each client: usename, password, group memberships, and email. The Authelia site administrator is responsible for creating user profiles with this infomation. There is no user "signup" portal. The user may change their password at any time after creation.

The database requires a working [SMTP]({{ site.data.links.smtp }}) in order to function properly. Email is sent from Authelia's host server to the client during the process of password changes and two-factor registration. Please see [SMTP Mail Relay Setup](/smtp_relay) for instructions on how to setup an email SMTP server that will forward email through [Gmail]({{ site.data.links.gmail }}) if necessary.

The Authelia User Database is formatted as [LDAP]({{ site.data.links.LDAP }}) or as a simple file. To simply setup of the Authelia server this guide will use the "file" method. It would be possible to use an existing LDAP server or create a new [Docker]({{ site.data.links.docker }}) instance of an LDAP server if required.

## Authelia User Database Setup

Please see the complete schematic of the [directory tree](/directory_tree) for the layout of paths that will created on the host server. This section will create the /etc/authelia/config/users.yml file.

{% capture text %}{% include become_root_warning.md %}{% endcapture %}{{ text | markdownify }}

Please create the following directory path:

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">/etc/authelia/config</div>
    <div class="growtalltxt">
      {% capture text %}{% include create-authelia-config-directory-structure.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

An example users.yml file is shown below. You will need to create this file and populate it with data relevant to your particular setup.  Groups names can be arbitray. Ensure the email address is valid. Password creation is discussed below.

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">Sample /etc/authelia/config/users.yml - MODIFY FOR YOUR SETUP</div>
    <div class="growtalltxt">
      {% capture text %}{% include users.yml.COMPLETE_EXAMPLE.txt.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

Hashed Passwords can be generated with following command: (Be sure to replace <password> with your actual password)

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">Password creation</div>
    <div class="growtalltxt">
      {% capture text %}{% include hash_password_command.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

Be sure to take the generated hashed password and place it within the users.yml file.  A password should be generated for each user within the users.yml file.  Users may change this password later if desired.

{% comment %}
--- Un-needed info
The ACL  can set a default policy for each site (bypass or deny all), and also limit access to sites on a username or group basis.  Rules can be mixed and matched to form a access list for each domain.
{% endcomment %}
