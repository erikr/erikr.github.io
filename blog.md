---
layout: post
permalink: /blog/
title: "Blog"
---

<div class="row">

{% for post in site.posts %}
{% if post.tags contains 'draft' %}
{% else %}

<a href="{{ post.url }}">{{ post.title }}</a><br>

{% endif %}
{% endfor %}

</div>
