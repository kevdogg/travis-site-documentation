---
layout: default
title: Pre-requisites
permalink: /pre-requisites/
nav_order: 4
---

# Checklist
{: .no_toc}

- [ ] Obtain a FQDN for each &nbsp; **_"Protected Domain"_** &nbsp; and {{site.data.links.authelia-domain}} 
- [ ] Add {{site.data.links.authelia-domain}} to your DNS records
- [ ] Obtain SSL certificates for {{site.data.links.authelia-domain}} and for Protected Domains 
- [ ] Configure Router to Open Ports 80/443 and forward to packets to server running reverse proxy
- [ ] Configure host server running reverse proxy to forward packets to docker (check kernel and network settings)
- [ ] Adjust firewall settings on router and server running reverse proxy to allow for incoming traffic.


### Syntax
{: .no_toc}

This document will often make reference to variable or locations which are surrounded by _<.....>._ This is an instruction to substitute the your actual name or variable value in this location. This __includes__ substitution of the brackets and all characters in between the brackets. 

For example, the value {{site.data.links.authelia-domain}} is an instruction for you to substitute your actual domain name for the bracketed portion - ie - <br>
    &emsp; _authelia.google.com_ or<br> 
    &emsp; _authelia.yahoo.com_ or<br>
    &emsp; _authelia.YOUR_DOMAIN_NAME.com_

# Details: 
{: .no_toc}

- Fully Qualified Domain Name
{: toc}

## Fully Qualified Domain Name

The User must have a Fully Qualified Domain Name. Domain Names are CHEAP!!!. There are many Domain Name Registrars where it's possible to obtain and lease a domain name. There are also services that offer domain names for free (ie [no-ip.com]({{site.data.links.no-ip}})), however these are usually subdomain rather than top-level domain names (tld's). I would recommend a tld domain if possible however a free domain name could also be used.

## Ability to Access and Alter DNS records

DNS providers associate your Domain Name with a server IP address. In many cases a DNS registrar may be the same provider the offers DNS services for your domain. 

For purposes of this guide you must have the ability to add a domain name to your DNS records that will resolve to the server IP where authelia and your reverse proxy is located. For purposes of this guide, I will refer to **{{site.data.links.authelia-domain}}** as the name associated with your authelia installation. \<domain.com>=your individual domain name.

Please add an entry for  with your DNS provider in order to complete the installation successfully. 

I personally use [Cloudflare]({{site.data.links.cloudflare}}) as my DNS provider.  The service is free, quick, and changes quickly propogate after DNS records are offered.

## Ability to Obtain SSL Certs for Domain

Although this guide does not specifically provide instructions how to obtain SSL certs, it requires that your have:

    1. A SSL certificate for your **Protected Domain(s) or Subdomain(s)**
    2. An SSL certificate will be need to be obtained for the 
       Authelia Portal - authelia.<domain.com>.

I personally recommend [Let's Encrypt]({{site.data.links.lets_encrypt}}) for SSL certificates.  The service is reliable and free with the condition you must renew your certificates every 90 days.

I also recommend the [acme.sh]({{site.data.links.acme}}) or [cerbot]({{site.data.links.certbot}}) client software to obtain and automatically renew your [Let's Encrypt]({{site.data.links.lets_encrypt}}) SSL certificates.  This guide unfortunately does not cover how to use these programs to obtain the certificates or how to configure these programs for automated renewal of the certificates.

## Linux Server or Virtualized Linux Server

All configurations of this guide were tested on an [Arch Linux]({{site.data.links.arch_linux}}) server. Instructions of this guide should be applicable with very minor modifications to any linux system. Linux was chosen because of its ability to run docker containers.  I personally run my setup within a byhve virtual machine (VM) running on [FreeNAS]({{site.data.links.freenas}}) which runs on [FreeBSD]({{site.data.links.freebsd}}). Any system capable of virtualizing a linux installation ([KVM]({{site.data.links.kvm}}), [Proxmox]({{site.data.links.proxmox}}), [Xen]({{site.data.links.xen}}), [XCP-NG]({{site.data.links.xcp-ng}}), [VirtualBox]({{site.data.links.virtual_box}}), etc) should be able to run Authelia via instructions in this guide

## Basic Knowledge of How to Configure a Reverse Proxy for Domain

This guide focuses on how to configure [Nginx]({{site.data.links.nginx}}) for authelia. There are many reverse proxies which could be used ([Traefik]({{site.data.links.traefik}}), [Caddy]({{site.data.links.caddy}}), [HAProxy]({{site.data.links.haproxy}}), etc.) Configuration of the headers for each reverse proxy will be sligthly different.  [Nginx]({{site.data.links.nginx}}) is the only system I've tested with [Authelia]({{site.data.links.authelia}}).  [Authelia]({{site.data.links.authelia}}) also officially supports the [Traefik]({{site.data.links.traefik}}) revere proxy.

I'm assuming those interested in using this authentication scheme have already configured their own website(s) using either [Nginx]({{site.data.links.nginx}})/[Apache]({{site.data.links.apache}}). If you have some knowledge of how to work with nginx and provide https access to a virtual domain, this guide should be straightfowrard. 

This guide will provide instructions how to setup **{{site.data.links.authelia-domain}}** as a virtual domain on nginx.


## Adjust firewall ports to allow for ingress of traffic on ports 80/443y

Firewall ports may need to be opened on ports 80/443 on the computer running the [Nginx]({{site.data.links.nginx}})/[Authelia]({{site.data.links.authelia}}) applications. Please check your configuration for [iptables]({{site.data.links.iptables}}) or [ufw]({{site.data.links.ufw}}) if you are using such tools on your linux distribution.

If you are self-hosting, your router settings will need to be adjusted to forward ports to 80/443 to the internal server running the reverse proxy.  With some routers, (i.e. [pfsense]({{ site.data.links.pfsense}})) the firewall may also be need configured to allow for passage of traffic on port 80/443 to the host system.
## Kernel level port forwarding 
The host system will need to be configured to allow kernel-level packet forwarding to the docker network and to the instance of [authelia]({{site.data.links.authelia}}) which runs within the authelial docker container.  Ipv4 forwarding needs likely needs to be:
  - Enabled within the system kernel [kernel_ipv4_activation]({{site.data.links.ipv4_forward_arch_linux}}) and
  - **Possibly Enabled** within the network settings if using [systemd-networkd]({{site.data.links.systemd_networkd_arch_linux}}), (not including netpland), An example systemd-networkd configuration file is listed below which is usually located within /etc/systemd/network/[^1],[^2],[^3]


###### Reference File 20-wired.network

```bash
[Match]
#Name=enp0s4
Name=eth0

[Network]
DHCP=ipv4
DNS=10.0.1.1
IPForward=kernel
```

## Docker nuances with iptables/ufw
The docker daemon directly manipulates the host's iptables. If using ufw as a frontend for iptables, this behavior may not be wanted or favorable since it may expose host services running on the same ports as the forwarded docker ports. Please consult [ufw-docker-discussion]({{site.data.links.ufw_docker_discussion}}) for discussion on how to possibly remedy this situation. I personally have not tested these instructions, however if running on a production server, these instructions may be helpful. 


[^1]: [Systemd Networking]({{site.data.links.systemd_networkd_arch_linux}})
[^2]: [Ubuntu Instructions for systemd-networkd if not using netplan.d]({{site.data.links.ubuntu_systemd_ipv4_forwarding}})
[^3]: [Systemd-Networkd Example Configuration]({{ site.baseurl }}{% link assets/raw/20-wired.network.txt %})


{% comment %}
<ol>
{% for prerequisites in site.data.prerequisites %}
  <li>
    <a href="#{{ prerequisites.condition }}">{{ prerequisites.condition }}
    </a>
  </li>
{% endfor %}
</ol>
{% endcomment %}
