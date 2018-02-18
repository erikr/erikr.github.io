---
layout: page
permalink: /blog/
title: "Blog"
---

<div class="row">

{% for post in site.posts %}

<a href="{{ post.url }}">{{ post.title }}</a><br>

{% endfor %}

</div>