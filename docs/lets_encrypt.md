---
layout: default
title: Let's Encrypt
tags: ["SSL", "authelia"]
permalink: /lets_encrypt/
nav_order: 170
---
<div class="container1x2">
  <div class="item">
    <img class="imgfitdiv" src="/assets/images/LetsEncryptLogo.png" alt="Let's Encrypt Logo">
  </div>
  <div class="item"> 
    <img class="imgfitdive" src="/assets/images/CloudFlare.jpg" alt="CloudFlare Logo">
  </div>
</div>

## Let's Encrypt SSL Certificates

[Let's Encrypt]({{ site.data.links.lets_encrypt }}) provides users with free SSL certificates with a limitation that the certificates have to be renewed every 90 days. Because of this limitation it is recommended that an automated renewal process be utilized to automate the renewal process. ACME Clients that are recommended for obtaining and renewing certificat include [CertBot]({{ site.data.links.certbot }}) or [acme.sh]({{ site.data.links.acme}}).

### Let's Encrypt Validation Process

[Let's Encrypt]({{ site.data.links.lets_encrypt }}) requires user validation when issuing and renewing certificates. Let's Encrypt needs to validate that control of the domain(s) for which are requesting certificates.  Let's Encrypt authenticates using [challenges]({{ site.data.links.lets_encrypt_challenge_types }}). "Challenge Types" include either HTTP challenge of DNS challenge.

HTTP challenge requires a file to be temporarily written/read on the requesting webserver. DNS challenge requires a temporary TXT record be added to your domain's DNS records. Various [DNS providers]({{ site.data.links.lets_encrypt_compatibled_dns_providers }}) are compatible with Let's Encypt.  Of these two methods, I have found DNS-challenge to be more robust. 

This guide will provide instructions on how to obtain SSL certificates through [Let's Encrypt]({{ site.data.links.lets_encrypt }}). The DNS-challenge method will be used with [Cloudflare]({{site.data.links.cloudflare}}) as the DNS provider.  [acme.sh]({{site.data.links.acme}}) will be utilized as the ACME client.  Additionally example [systemd]({{site.data.links.systemd}}) timer and service files will be provided if users would like to use this method to automated system renewals.  [Crontab]({{site.data.links.crontab}}) or [Fcron]({{site.data.links.fcron}}) entries could be utilized as alternatives.



## Let's Encrypt Validation with DNS Challenge  

For each domain requiring validaton, a corresponding A or CNAME record is required.
Usually the base level domain is considered the A record.  The A record is linked to an IP address.  The CNAME records are linked to the A record.  If utilizing a setup where one WAN IP Address is hosting multiple domains/subdomains, usually the A record is the basedomain and the CNAME recods are the subdomains of the basedomain  which point to the A record and the same IP addresss.

For example:

| Type  | Name                   | Content         |
|-------|------------------------|-----------------|
| A     | example.org            | 555.555.555.555 |
| CNAME | subdomain1.example.org | example.org     |
| CNAME | subdomain2.example.org | example.org     |

For every domain or subdomain that requires validation, there must be an A or CNAME entry.

For the domains listed above, it is possible to:

  - Obtain 3 separate SSL certificates.  Each SSL certificate contains only the name of itsh respective domain.
  - Obtain 1 SSL certificate which contains the name of all three domains within the same certificate

Some refer to option 2 as obtaining a ["SAN Certificate"]({{site.data.links.LE_SAN_Certs_Ubuntu}}).
Whether you obtain multiple certificates or one certificate associated with multiple domains is a matter of personal preference.

If following this guide for [Authelia]({{site.data.links.authelia}}) setup, you must at least have either 2 certs, or 1 cert with 2 listed domains.  The domains should be __\<domain.com\>__ and __authelia.\<domain.com\>__

Please note that each subdomain does not need to be publicly accessible.  It is possible to use subdomain2.example.org internally and for this particular domain to never be reachable from outside the Local Area Network.  

## Acme.sh Setup 

{% capture text %}{% include become_root_warning.md %}{% endcapture %}{{ text | markdownify }}

{% capture text %}{% include variable_substitution_warning.md  %}{% endcapture %}{{ text | markdownify }}


Official [acme.sh]({{site.data.links.acme_setup}}) installation instructions.

{% capture code %}apt install git{% endcapture %}
{% include code.html header="Ubuntu" code=code lang="bash" %}
{% capture code %}pacman -S git cronie{% endcapture %}
{% include code.html header="Arch" code=code lang="bash" %}

{% capture code %}{% include acme_setup.txt %}{% endcapture %}
{% include code.html header="acme.sh Installation - All Linux Versions" code=code lang="bash" %}

### Acme.sh and Cloudflare

When working with [Cloudflare]({{site.data.links.cloudflare}}), [acme.sh]({{site.data.links.acme_setup}}) requires either:
  
  - The Cloudflare Global API Key Option __----OR----__ 
  - The Cloudflare API Token Method Option
{: .fw-500 .text-green-000 .fs-4 }

| Cloudflare Global API Key Option | Cloudflare API Token Method Option          |
|----------------------------------|---------------------------------------------|
| Cloudflare Key -> CF_KEY         | Cloudflare Token -> CF_Token                |
| Cloudflare Email -> CF_EMAIL     | Cloudflare Account ID -> CF_Account_ID      |

You need criteria from either of the two columns. The Cloudfare Global API Key Option is the Legacy Option whereas the Cloudflare API Token Method Option is considered the newer option as options may be more finely tuned per user app.

When logged in [Cloudflare]({{site.data.links.cloudflare}}) and after selecting your particular domain, these criteria can be found at the following locations:

The __Cloudflare Key__ is also known as the __Cloudflare Global API Key__.  This is found under the My Profile -> API Tokens -> Global API Key Menu.

The __Cloudflare Email__ is the email that was used when registering with CloudFlare when the account was created.

The __Cloudflare Account ID__ can be found either:
  - At the Bottom Right Column under "Account ID" when visiting the Overview Page for the specific DOMAIN
  - Within the Browser URL bar -- It the long alphanumeric code listed between https://dash.cloudflare.com/__abkdkd-long-alpha-numeric-code-here-akk23939__/\<domain_name\>

The __Cloudflare Token__ Needs to be Generated:
  - Goto My Profile -> API Tokens
  - Select Create Token
  - Give the Token a Name
  - 2 Permissions are Needed:
    - Zone Zone Read
    - Zone DNS Edit
  - Zone Resources - Include All Zones
  - Copy the resultant long number given as it will not be able to retrieved again

You will need to export either pair of criteria as environmental variables for acme.sh to utilize their values.

{% capture code %}{% include cloudflare_global_api_key_option.txt %}{% endcapture %}
{% include code.html header="Cloudflare Global API Key Option - All Linux Versions" code=code lang="bash" %}
{% capture code %}{% include cloudflare_api_token_method_option.txt %}{% endcapture %}
{% include code.html header="Cloudflare API Token Method Optioni - All Linux Versions" code=code lang="bash" %}


## Let's Encrypt Certificate Generation with acme.sh and DNS Challenge with Cloudflare

For illustrative purposes, the Let's Encrypt Certificates will be stored in `/etc/letsencrypt/<domain_name>`. Please modify instructions below for your setup.

{% capture code %}{% include lets_encrypt_cert_installation.txt %}{% endcapture %}
{% include code.html header="Let's Encrypt Certificate Installation One Domain - All Linux Versions" code=code lang="bash" %}

The reloadcmd is optional. In many cases however after renewing certificates you may want to reload  an application utilizing the certificates.  In this particular application, ngnix is the reverse proxy utilizing the certifcates for the various domains.  To automatically reload the new certificates within Nginx, the reloadcmd would be `--reloadcmd "nginx -s reload"`. If multiple reload commands are needed, put them in a shell script and have the reloadcmd call the shell script.

## Let's Encrypt Certificate Auto Renewal

By default, acme.sh will create an entry into the root crontab. Certificate renewal will be attempted nightly.  The crontab entry will appear similar to:

{% capture code %}{% include root_crontab_acme.txt %}{% endcapture %}
{% include code.html header="Acme.sh Default crontab entry for LE Certificate Renewal" code=code lang="bash" %}

## Optional Renewal Implementation with Systemd Timers rather than using crontab

I've personally attempted to convert most of my crontab entries into corresponding [systemd]({{ site.data.links.systemd }}) unit files and timers. 

I first commented out the acme entry contained within the crontab file, and then created two files:
  - acme_letsencrypt@.service
  - acme-letsencrypt@.timer

Both these files for me were placed within /etc/systemd/system however please check your distribution for the exact location.

{% capture code %}{% include acme_letsencrypt.service %}{% endcapture %}
{% include code.html header="/etc/systemd/system/acme_letsencrypt@.service" code=code lang="bash" %}

{% capture code %}{% include acme_letsencrypt.timer %}{% endcapture %}
{% include code.html header="/etc/systemd/system/acme_letsencrypt@.timer" code=code lang="bash" %}

With this setup, the timer will start the service file to renew the certificate(s).

Timers can be enable to start at boot and started by:

```bash
systemd enable acme_letsencrypt@<domain_name>.timer
systemd start acme_letsencrypt@<domain_name>.timer
```

So for example if domain_name=example.org

```bash
systemd enable acme_letsencrypt@example.org.timer
systemd start acme_letsencrypt@example.org.timer
```

Use `systemctl list-timers` to see status of timers, last run, etc.

