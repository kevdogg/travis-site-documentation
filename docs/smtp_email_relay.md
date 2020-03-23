---
layout: default
title: SMTP Mail Relay Setup 
tags: ["smtp", "mail", "authelia"]
permalink: /smtp_relay/
nav_order: 120
---

<div class="imagerow">
  <div class="imagecol">
    <div class="growtall"><img src="/assets/images/PostfixLogo.png" alt="Authelia Login Page"></div>
    <div>Postfix</div>
  </div>
  <div class="imagecol">
    <div class="growtall"><img src="/assets/images/GmailLogo.jpg" alt="Authelia"></div>
    <div>Gmail</div>
  </div>
</div>

## SMTP Email Relay Setup
{: .no_toc }

[Authelia]({{ site.data.links.authelia }}) requires sending email to users when setting up two-factor authentication.  Unless there is already a mail server or mail relay running within your domain, this guide is a quick primer on how to setup a local mail relay to forward e-mails through [Gmail's]({{ site.data.links.gmail }}) smtp servers.  This guide demonstrates how to setup [Postfix]({{ site.data.links.postfix }}) as a [Mail Transfer Agent (MTA)]({{ site.data.links.mta }}) in order to send mail from the Authelia's host server.


  - Package Installation
  {: toc}


{% capture text %}{% include become_root_warning.md %}{% endcapture %}{{ text | markdownify }}

### Package Installation
Install postfix and accompanying packages for your linux distribution. Commands for Ubuntu and Arch are listed below. If you are using other linux distributions, package names may be different. Consult your distribution's repository pages to try to find the corresponding package.

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">Ubuntu</div>
    <div class="growtalltxt">
      {% capture text %}{% include ubuntu-smtp-packages.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>
<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">Arch Linux</div>
    <div class="growtalltxt">
      {% capture text %}{% include arch-smtp-packages.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

### Postfix Configuration
When installing the postfix package, you'll be greeted with a screen similar to the following:

<div class="rowimage">
  <div class="imagecol">
    <div class="growtall">
  <img src="/assets/images/PostfixSetup.png" alt="Postfix Setup">
    </div>
  </div>
</div>

&nbsp;

It's Ok at this point to select any option, as I'll be providing a very basic postfix main.cf file below.

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">All Linux Versions</div>
    <div class="growtalltxt">
      {% capture text %}{% include postfix_config_1.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

Paste the contents of main.cf[^1] below.  This will give you a very basic mail relay setup in which only mail originating from the host machine will be forwarded to Gmail.

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">/etc/postfix/main.cf</div>
    <div class="growtalltxt">
      {% capture text %}{% include main.cf.txt.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

**IMPORTANT**

Please be sure to **add the correct values for myhostname and the mydomain variables** and **verify the /etc/ssl/certs/ca-certificates.crt file actually exists**.

Create an /etc/postfix/aliases file
<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">All Linux Versions</div>
    <div class="growtalltxt">
      {% capture text %}{% include postfix_config_2.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

Use the following example aliases file example to build your aliases file. Make sure to correct for the **\<variables\>** listed within the file.

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">/etc/postfix/main.cf</div>
    <div class="growtalltxt">
      {% capture text %}{% include aliases.txt.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

After creating /etc/aliases, run the following two commands to build the alias database

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">All Linux Versions</div>
    <div class="growtalltxt">
      {% capture text %}{% include postfix_config_3.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

### Gmail App Specific Password Setup
Within [Gmail]({{ site.data.links.gmail }}) please create an [App Specific Password]({{ site.data.links.gmail_app_specific_password }}) for postfix.  You will need the generated password for the next step.

### Using Gmail's App Specific Password within Postfix

Create the sasl_passwd_GMAIL_587 and sasl_passwd_GMAIL_465 files. 587 and 465 refer to ports whereby mail can be relayed via [SMTP/TLS or smtps]({{ site.data.links.smtp-tls_vs_smtps }}). I've included both options here to be thorough.  Ensure to replace the **\<variables\>** with the correct information. The <app_specific_password> was generated in the previous step.

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">All Linux Versions - /etc/postfix/sasl_passwd_GMAIL_587</div>
    <div class="growtalltxt">
      {% capture text %}{% include sasl_passwd_GMAIL_587 %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

Hash the Gmail passwd files created above and add to database


<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">All Linux Versions - /etc/postfix/sasl_passwd_GMAIL_465</div>
    <div class="growtalltxt">
      {% capture text %}{% include sasl_passwd_GMAIL_465 %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">All Linux Versions</div>
    <div class="growtalltxt">
      {% capture text %}{% include postfix_config_4.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

### Enable/Start Postfix 

Start and enable the postfix service

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">Linux Versions using systemd</div>
    <div class="growtalltxt">
      {% capture text %}{% include postfix_config_5.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

### Test Configuration

Test Configuration by sending a test email

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">All Linux Versions</div>
    <div class="growtalltxt">
      {% capture text %}{% include postfix_config_6.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

Replace recipient_user@domain.com with the email address to receive the test email, and confirm that the email was received.

### Debugging 

If there are errors in the process trying to send email, please look in the following locations to see if an error message may give and indication of the problem:
  1. /var/log/mail.log
  2. /var/log/mail.err
  3. journalctl -u postfix.service

  You may need to increase the logging level within the master.cf file to generate more output. Please consult [Postfix documentation]({{ site.data.links.postfix_debugging }}) for some useful tips for debugging postfix.

### Port 465 vs Port 587

Control of Port Type is made through modification within the /etc/postfix/main.cf file. You should configure postfix to send mail using one but not both ports. Please make the following modifications to main.cf to reflect your port choice

<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">Port 465</div>
    <div class="growtalltxt">
      {% capture text %}{% include port465.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
  <div class="imagecoltxt">
    <div class="header">Port 587</div>
    <div class="growtalltxt">
      {% capture text %}{% include port587.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>


[^1]: [main.cf](/assets/raw/main.cf.txt)
