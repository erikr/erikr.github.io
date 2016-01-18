---
layout: page
permalink: /blog/
title: ""
---

{% for post in site.posts %}
<a href='{{ post.url }}'>{{ post.title }}</a>
<br>
{% endfor %}
