---
layout: page
title: Recap
permalink: /blog/categories/recap/
category: recap
---

<h5> Posts by Category : {{ page.title }} </h5>

<div class="card">
{% for post in site.categories[page.category] %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>