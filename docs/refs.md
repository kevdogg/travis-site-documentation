---
layout: page
title: Configuration Files
permalink: /configuration_files/
nav_order: 8 
---

# Configuation Files

{% assign files = site.data.reference_files_db %}
{% assign key = 'group' %}
{% assign groups = files | map: key | uniq %}

{% for group in groups %}
* ## {{ group | capitalize }}
  {% for member in files %}
    {% if member.group == group %}
      {% assign key = member.link_name %}
      {% assign URL = site.data.links | map: key | first | prepend: site.baseurl %}
      {% assign raw_URL = URL | replace: "files", "raw" | append: ".txt"  %}
  * [{{ member.name }}]({{ URL }}) - {{ member.description }} - <sup> [Raw File]({{ raw_URL }})</sup>
    {% endif %}
  {% endfor %}
{% endfor %}

{% comment %}
key = {{ key }}
files = {{ files }}
groups = {{ groups }}

{% assign grouped_files = files | group_by: 'group' | sort: 'group' %}
grouped_files = {{ grouped_files}}
{% for group in grouped_files %}
* ## {{ group.name | capitalize }}
  {% for item in group.items  %}
    {% assign key = item.link_name %}
    {% assign URL = site.data.links | map: key | first | prepend: site.baseurl %}
  * [{{ item.name }}]({{ URL }}) - {{ item.description }}
  * <a mimetype="text/plain" type="text/plain"  href="{{ URL }}">{{ item.name }}</a> - {{ item.description }}
  {% endfor %}
Group:{{ group }}<br>
{% endfor %}

{% assign subArraySize = 2 %}

{% assign myArray = "one,two,three,four,five,six,seven" | split: "," %}
myArray = {{ myArray | inspect }}

{% assign multiArray = "" | split: "/" %}
multiArray = {{ multiArray | inspect }}

{% for element in myArray %}
  looping in myArray - forloop.index = {{ forloop.index }}

  {% assign reminder = forloop.index | modulo: subArraySize %}
  remainder : {{ reminder | inspect }}

  {% if reminder == 1 %}
  create a new empty sub array
  {% assign subArray = "" | split: "/" %}
  subArray = {{ subArray | inspect }}
  {% endif %}

  push current element in subArray
  {% assign subArray = subArray | push: element %}
  subArray = {{ subArray | inspect }}

  {% if reminder == 0 or forloop.last %}
  push subArray in multiArray if subarray length is
    {% assign multiArray = multiArray | push: subArray %}
  multiArray = {{ multiArray | inspect }}
  {% endif %}
{% endfor %}}

{% assign gA = "" | split: "/" %}
{% for group in groups %}
  {% assign name = "sub1" %}
  {% assign subArray = "" | split: "/" %}
  {% assign subArray = subArray | push: group %}
  subArray = {{ subArray | inspect }}
  {% assign gA = gA | push: subArray %}
{% endfor %}
gA = {{ gA | inspect }}


{% for member in files %}
member= {{ member }}<br>
member.group = {{ member.group }}<br>

  {% for group in groups %}
    {% if group == member.group %}
      Match found: {{ member.group }}
      index: {{ forloop.index0 }}
      {% break %}
    {% endif %}
  {% endfor %}

hello 

{% endfor %}

{% for group in groups %}
  {% assign members = files | map: group %}
  members={{ members }}
  {% for member in members %}
    Name: {{ member.name }}
    Group: {{ member.group }}
  {% endfor %}
{% endfor %}

{% for item in groups %}
item={{ item }}<br>
{% endfor %}

{% for listing in site.data.reference_files_db %}

listing = {{ listing }}

{% endfor %}
{% endcomment %}
