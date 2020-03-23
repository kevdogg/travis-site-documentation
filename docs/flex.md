---
layout: default
title: Flexbox
tags: ["smtp", "mail", "authelia"]
permalink: /flexbox/
nav_order: 150
---


<div class="imagerowtxt">
  <div class="imagecoltxt">
    <div class="header">Ubuntu</div>
    <div class="growtalltxt">
      {% capture text %}{% include ubuntu-smtp-packages.md %}{% endcapture %}
      {{ text | markdownify }}
    </div>
  </div>
</div>

{% comment %}
{% capture filetext %}{% include bash_text.txt %}{% endcapture %}
{% capture markdowncode %}```bash
{{ filetext }}
```{% endcapture %}
{% assign code="This is code" %}↲
<div class="containerlayout">
  <div class="itemlayout">
      Hello
  </div>
  <div class="itemlayout">
{% include code.html code=code lang="bash" %}
  </div>
  <div class="itemlayout">
      {{ markdowncode | markdownify }}
  </div>
</div>
{% endcomment %}

{% capture code %}{% include bash_text.txt %}{% endcapture %}
{% include code.html header="bash_text.txt" code=code lang="bash" %}
