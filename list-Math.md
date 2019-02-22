---
layout: page
title: Math
comments: true
---

{% for post in site.posts %}
{% if post.tags contains 'math' %}
  * {{ post.date | date_to_string }} / [ {{ post.title }} ]({{ post.url }})
{% endif %}
{% endfor %}
