---
layout: page
permalink: /blog/
title: ""
---

{% for post in site.posts %}
{% if post.tags contains 'draft' %}
{% else %}
<p><a href='{{ post.url }}'>{{ post.title }}</a></p>
{% endif %}
{% endfor %}
