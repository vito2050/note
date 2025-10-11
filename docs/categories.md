---
layout: default
title: 分类
permalink: /categories.html
---

# 分类

{% assign docs_pages = site.pages | where_exp:'p','p.path contains "docs/"' %}
{% assign categories = docs_pages | map: 'category' | compact | uniq | sort %}

{% if categories.size == 0 %}
暂无分类。
{% else %}
<ul>
{% for c in categories %}
  <li><a href="#{{ c | slugify }}">{{ c }}</a></li>
{% endfor %}
</ul>

{% for c in categories %}
### {{ c }}
<ul>
{% for p in docs_pages %}
  {% if p.category == c %}
  <li><a href="{{ p.url | relative_url }}">{{ p.title }}</a></li>
  {% endif %}
{% endfor %}
</ul>
{% endfor %}
{% endif %}
