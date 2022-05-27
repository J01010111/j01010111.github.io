---
layout: page
title: Hacking Tools
permalink: /blog/categories/HackingTools/
---

<h4> Posts por categorías : {{ page.title }} </h4>

<div class="card">
{% for post in site.categories.HackingTools %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>
