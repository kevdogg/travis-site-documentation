---
layout: page
title: Reference Files
permalink: /download/
nav_order: 11
---
BaseURL: {{ site.baseurl }}

# Reference Files:

* ## Authelia Configuration Files
  * [configuration.yml]({{ site.baseurl }}/assets/files/configuration.yml) - Main Authelia Configuration File
  * [.env]({{ site.baseurl }}/assets/files/.env) - Authelia Environment Variables and Passwords
  * [users.yml]({{ site.baseurl }}/assets/files/users.yml) - Example User Database File

* ## Authelia Nginx Configuration Files
  * [authelia.domain.com.cert.conf]({{ site.baseurl }}/assets/files/authelia.domain.com.cert.conf) - Let's Encrypt SSL parameters for Authelia Main Site
  * [authelia.domain.com.conf]({{ site.baseurl }}/assets/files/authelia.domain.com.conf) - Nginx Virtual Domain File for authelia.domain.com
  * [authelia.location.conf]({{ site.baseurl }}/assets/files/authelia.location.conf) - Authelia Headers to Protect BackendDomain
  * [authelia_auth.conf]({{ site.baseurl }}/assets/files/authelia_auth.conf) - Authelia Authentication Headers for Backend Domain
  * [authelia.proxy.con]({{ site.baseurl }}/assets/files/authelia_proxy.conf) - Authelia Reverse Proxy Headers

* ## Example Nginx Configuration Files
  * [ssl-modern-params.conf]({{ site.baseurl }}/assets/files/ssl-modern-params.conf) - Modern Nginx SSL Configuration
  * [ssl-params.conf]({{ site.baseurl }}/assets/files/ssl-params.conf) - Intermediate Nginx SSL Configuration
  * [nginx.conf]({{ site.baseurl }}/assets/files/nginx.conf) - Basic Nginx Main Configuration

* ## Docker Configuration Files
  * [docker-compose.yml]({{ site.baseurl }}/assets/files/docker-compose.yml) - Docker Compose File for All Docker Related Images

* ## Systemd Service Files
  * [docker-compose@.service]({{ site.baseurl }}/assets/files/docker-compose@.service) - Docker Compose systemd Service File
  * [docker-compose-reload@.service]({{ site.baseurl }}/assets/files/docker-compose-reload@.service) - Docker Compose systemd Reload Service File to Automatically Reload Docker Images
  * [docker-compose-reload@.timer]({{ site.baseurl }}/assets/files/docker-compose-reload@.timer) - Docker Compose systemd Reload Timer File to Automatically Reload Docker images

  ## Systemd-networkd

