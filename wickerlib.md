---
layout: page
title: Wickerlib
permalink: /wickerlib/
---

Wickerlib is a thing I built. 

{% for post in site.wickerlib %}
  <a href="{{ post.url }}">{{ post.title }}</a><br />
  {{ post.description }} 
{% endfor %}
