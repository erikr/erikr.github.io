---
layout: post
permalink: /blog/
title: "Blog"
---

---

{% for post in site.posts %}
{% if post.tags contains 'draft' %}
{% else %}
<a href='{{ post.url }}'>{{ post.title }}</a>
{% endif %}
{% endfor %}
