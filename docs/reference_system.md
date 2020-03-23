---
layout: page
title: Reference System
permalink: /reference_system/
nav_order: 3
---

# Reference System Specification

<ul>
{% for system in site.data.system_specs %}
  {% assign key = system.link_name %}
  {% assign resource = site.data.links | map: key %}
  {% for item in resource limit:1 %}
    <li class="space">
    {% if system.comment %}
      {% assign dash = "-" %}
    {% else %}
      {% assign dash = "" %}
    {% endif %}
    <a href="{{ item }}">{{ system.name }}</a> Version: {{ system.version }}<p class="space">{{ dash }} {{ system.comment }}</p>
    </li>
  {% endfor %}
{% endfor %}
</ul>

## Configuration

### Host System Setup
  * Nginx Reverse Proxy
  * Docker - controlled via Docker Compose

### Docker
All the docker images are hosted on [Docker Hub]({{ system.data.link.docker_hub }})

<ul>
{% for system in site.data.docker_container_list %}
  {% assign key = system.link_name %}
  {% assign resource = site.data.links | map: key %}
  {% for item in resource limit:1 %}
    <li class="space">
    {% if system.comment %}
      {% assign dash = "Comment: " %}
    {% else %}
      {% assign dash = "" %}
    {% endif %}
    <a href="{{ item }}">{{ system.name }}</a><p class="space">version: {{ system.version }}</p><p class="space">{{ dash }}{{ system.comment }}</p>
    </li>
  {% endfor %}
{% endfor %}
</ul>

### Volumes on Host System Mounted within Docker Containers

| Host Directory/File | Container | Container Mount Point |
|:--------------------|:----------|:----------------------|
| /etc/authelia/config/configuration.yml | authelia | /etc/authelia/configuration.yml |
| /etc/authelia/config/users.yml | authelia | /etc/authelia/users.yml |
| /var/data/db/ | mysql | /config |

[Host Directory Tree]({{ site.baseurl }}{% link docs/directory_tree.md %})
