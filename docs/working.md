---
layout: page 
title: work
permalink: /working/
nav_order: 15
---

# Checklist
{: .no_toc}

- [ ] Obtain a FQDN for each &nbsp; **_"Protected Domain"_** &nbsp; and {{site.data.links.authelia-domain}} 
- [ ] Add {{site.data.links.authelia-domain}} to your DNS records
- [ ] Obtain SSL certificates for {{site.data.links.authelia-domain}} and for Protected Domains 
- [ ] Configure Router to Open Ports 80/443 and forward to packets to server running reverse proxy
- [ ] Configure server running reverse proxy to forward packets
- [ ] Adjust firewall settings on router and server running reverse proxy to allow for incoming traffic.
