---
layout: default 
title: Overview
tags: ["overview", "authelia"]
permalink: /overview/
nav_order: 1
---

![Authelia logo](/assets/images/authelia-title.png)

[Authelia](https://https://github.com/authelia/authelia) is an open-source applicationthat provides 2-factor authentication and single sign-on (SSO) for web based applications.

[Authelia](https://https://github.com/authelia/authelia) is a Docker-based application that sits between a reverse proxy and a web application. Authelia provides "frontend" authentication for the "backend" web application/service.
  
  The authentication scheme(s) which can be utilized are a username/password and optionally coupled with a two-factor mechanism -- [Google Authenticator](https://en.wikipedia.org/wiki/Google_Authenticator), [Duo Push Notifications](https://duo.com/product/multi-factor-authentication-mfa/authentication-methods/duo-push), or [YubiKey](https://www.yubico.com/).


<table id="third_width">
<tr>
  <td>
     <img class="imagesmall" src="/assets/images/GoogleAuthenticator.png" alt="Google_Authenticator Logo">
  </td>
  <td>
     <img class="imagesmall" src="/assets/images/DuoPushLogo.png"  alt="Dou Logo">
  </td>
  <td>
     <img class="imagesmall" src="/assets/images/YubiKey.jpg"  alt="Dou Logo">
  </td>
</tr>
<tr>
  <td class="third_width"><a href="https://en.wikipedia.org/wiki/Google_Authenticator">Google Authenticator</a></td>
  <td><a href="https://duo.com/product/multi-factor-authentication-mfa/authentication-methods/duo-push">Duo Push</a></td>
  <td><a href="https://www.yubico.com/">YubiKey</a></td>
</tr>
</table>


The purpose of this document is to explain how to setup and implement an [Authelia](https://https://github.com/authelia/authelia) portal for a backend application.

## Initial Requirements

It is assumed there is an existing framework in place in which the backend application is currently accessible behind a reverse proxy.  This framework would appear similar to:

![Initial Reverse Proxy Setup](/assets/images/Initial_Reverse_Proxy_Setup.png)


Instructions on how to configure a reverse proxy for your web application to fulfill the initial requirements are discussed in [here]({{ site.data.links.samuel_dowling_nginx_reverse_proxy }}).[^1]

Although _not an absolute requirement_, it is recommended SSL certificates are utilized for the hosted domain. 

## Completed Setup

After completing the steps in this guide, the framework should appear similar to:

![Final Reverse Proxy Authelia Setup](/assets/images/ReverseProxy_Final_Authelia_Setup.png)

## Guide Parameters and Limitations

### This guide will:
1. Discuss the Authelia Application
2. Configure a linux based [Nginx]({{ site.data.links.nginx }}) Reverse Proxy in order to use Authelia for frontend authentication
3. Configure a [Docker]({{ site.data.links.docker }}) Setup consisting of [authelia]({{ site.data.links.docker_authelia }}), [redis]({{ site.data.links.docker_redis }}) and [mariadb]({{ site.data.links.docker_mysql }}) images as required by [Authelia]({{ site.data.links.authelia }})
4. Discuss the concept of ***DNS Host Overrides*** and discuss setting up DNS Host Overrides at the *Host* or *Router* Level so host name resolution will work for the configured setup.


### This guide will not cover:
1. Router level port forwarding
2. Firewall configuration either at the Router or Host Level.

Given the initial starting point, I am assuming the end-user has configured router level port forwarding and router/host firewall in order to allow access to the hosted web application without the frontend [Authelia]({{ site.data.links.authelia }}) authentication scheme. 

## Compatibility

This guide should be applicable with slight modifications to all systems capable of running docker containers.

This guide was constructed using the following [reference system]({% link docs/reference_system.md %}) 

## Reference Files

All configuration files used in this guide may be found [here]({% link docs/refs.md %})

In configuring [Authelia](https://https://github.com/authelia/authelia), at least 2 or more sites need to be configured within the reverse proxy:
     
     1. The Authelia Portal
     2. One or more Protected Sites or Protected Domains/Subdomains/Folders
{: .px-10 .fs-2 }

&nbsp;

## The Authelia Portal
The Authelia Portal is a website where users will initially log in to register and activate the two factor authentication method.  Users will login with a given username/password and then register with the Google Authenticator app or Duo Push app.  Users are sent an email to confirm the registration during this process.

The Authelia Portal appears similar to:

<div class="rowimage">
  <div class="columnimage1x2">
  <img src="/assets/images/AutheliaSignIn2.png" alt="Authelia Portal" style="width:100%">
  </div>
  <div class="columnimage1x2">
  <img src="/assets/images/AutheliaSecurityKey2.png" alt="Authelia Portal" style="width:100%">
  </div>
</div>


## Protected Site(s)
Once registered, users will have the ability to access specific **Protected Sites or Domains** using their registered authentications scheme. When a user attempts to access the URL of the protected site, the user will be redirected to the authelia portal. Users will need to authenticate with a username/password along with either a Google Authenticator Code, Duo Push Notification or YubiKey if two-factor authentication is configured for the **Protected Site**. Once authenticated within authelia, the user will be redirected to the **Protected Site**. Below shows two screenshots for authenticating with a Goggle Authenticator code and a Duo Push Notification.


[^1]: [Samuel Dowling: HOW TO SET UP AN NGINX REVERSE PROXY WITH SSL TERMINATION]({{ site.data.links.samuel_dowling_nginx_reverse_proxy }})

{% comment %}
This documentation will often refer to the backend application(s) which uses [Authelia](https://https://github.com/authelia/authelia) for authentication as the **"Protected Site(s)"**
{:.fs-1} 



<div class="rowimage">
  <div class="columnimage1x2">
  <img src="/assets/images/OneTimePassword2.png" alt="Authelia Portal" style="width:100%">
  </div>
  <div class="columnimage1x2">
  <img src="/assets/images/PushNotification2.png" alt="Authelia Portal" style="width:100%">
  </div>
</div>

&nbsp;  

This Guide will provide a basic framework of how to setup and configure the Authelia Portal in order to protect and secure websites and web applications.

Hello

1. {:.fs-5} done
   1. {:.fs-5} Not done
{: .px-10 }

This guide serves as companion documentation in order to setup authelia and allow Authelia to protect web applications running behind a reverse proxy

Margi/Padding Section

Padding
{: .px-10 }

Margin 20
{: .mx-20 }

During markup
After markdown

This paragraph will have 2rem/32px of padding on the right and left at all screen sizes.
{: .px-6 }

.Authelia is a
The word color is blue{:id}{:.text-yellow-000} not *yellow*{:.underline}
Inline html <span class="text-yellow-000">not yellow</span>
Mardown *red*{: style="color: red"}

{% highlight bash %}
git push -u origin source
{% endhighlight %}

{: .text-yellow-000}
This is not yellow

<div class="img-container400">
 <img class="resize" src="/assets/images/AutheliaSignIn.png" alt="Authelia Portal">
<img class="resize" src="/assets/images/AutheliaSecurityKey.png" alt="Authelia Portal">
</div>
{% endcomment %}
