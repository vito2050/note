---
layout: default
title: 标签
permalink: /tags.html
---

# 标签

{% assign docs_pages = site.pages | where_exp:'p','p.path contains "docs/"' %}
{% assign tags = docs_pages | map:'tags' | join:',' | split:',' | uniq | sort %}

{% if tags.size == 0 %}
暂无标签。
{% else %}
<ul>
{% for t in tags %}
  {% if t and t != '' %}
  <li><a href="#{{ t | slugify }}">{{ t }}</a></li>
  {% endif %}
{% endfor %}
</ul>

{% for t in tags %}
  {% if t and t != '' %}
  ### {{ t }}
  <ul>
  {% for p in docs_pages %}
    {% if p.tags and p.tags contains t %}
    <li><a href="{{ p.url | relative_url }}">{{ p.title }}</a></li>
    {% endif %}
  {% endfor %}
  </ul>
  {% endif %}
{% endfor %}
{% endif %}
