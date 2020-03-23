---
layout: default 
title: Authelia specifics
tags: ["overview", "authelia"]
permalink: /authelia_specifics/
nav_order: 110
---

![Authelia logo](/assets/images/authelia-title.png)

Authelia is comprised of the following two components:

1. The Athelia Registration Portal
2. The Authelia Authentication Portal 

## The Authelia Registration Portal

The Authelia Registration Portal is a locally hosted web application where clients will initially log in and activate their desired two factor authentication method(s).  Clients will initially login with a username/password initially provided by the site administrator. Through the registration portal, users have the option of adding [Google Authenticator]({{ site.data.links.google_authenticator }}), [Duo Push]({{ site.data.links.duo_push }}) and/or [YubiKey]({{ site.data.links.yubikey }}) as two factor authentication schemes. Creation of this user database is discussed below. 

The Authelia Registration Portal appears similar to:

{% comment %}
<div class="rowimage">
  <div class="columnimage1x2">
  <img src="/assets/images/AutheliaSignIn2.png" alt="Authelia Portal" style="width:100%">
  </div>
  <div class="columnimage1x2">
  <img src="/assets/images/AutheliaSecurityKey2.png" alt="Authelia Portal" style="width:100%">
  </div>
</div>
{% endcomment %}

<div class="imagerow">
  <div class="imagecol">
  <img src="/assets/images/AutheliaSignIn2.png" alt="Authelia Sign In Portal">
  </div>
  <div class="imagecol">
  <img src="/assets/images/AutheliaSecurityKey2.png" alt="Authelia Security Key">
  </div>
</div>

## The Authelia Authentication Portal

In visiting domains/websites/applications that utilize [Authelia]({{ site.data.links.authelia }}) for frontend authentication, clients are redirected to the Authelia Authentication portal to complete the authentication process.  Clients are greeted with the familiar login screen as used with the registration portal. After logging in clients are directed to enter their second authentication credential if the domain/website/application was configured to require two-factor authentication. Domains/websites/applications can be configured through authelia to use single-factor authentication (Username/Password) with optional coupling of a two-factor authentication schemes ([Google Authenticator]({{ site.data.links.google_authenticator }}), [Duo Push]({{ site.data.links.duo_push }}) and/or [YubiKey]({{ site.data.links.yubikey }})). It is possible to setup multiple schemes in which the user may use one method as the second-factor authentication paramater.


The Authelia Authentication Portal appears similar to:

<div class="imagerow">
<div class="imagerow">
  <div class="imagecol">
    <div class="growtall"><img src="/assets/images/AutheliaSignIn.png" alt="Authelia Login Page"></div>
    <div>Authelia Login Page</div>
  </div>
  <div class="imagecol">
    <div class="growtall"><img src="/assets/images/AutheliaSelectAuthenticationScheme.png" alt="Authelia"></div>
    <div>Two Factor Method Choice</div>
  </div>
  <div class="imagecol">
    <div class="growtall"><img src="/assets/images/AtheliaOneTimePassword.png" alt="Authelia"></div>
    <div>Google Authentication</div>
  </div>
</div>
<div class="headertxt boldtxt centertxt">Authelia Two Factor Authentication with Google Authenticator</div>
</div>

