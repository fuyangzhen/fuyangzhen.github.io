---
layout: page
title: Memo
comments: true
---

{% for post in site.posts %}
{% if post.tags contains 'memo' or post.tags contains 'other' %}
  * {{ post.date | date_to_string }} / [ {{ post.title }} ]({{ post.url }})
{% endif %}
{% endfor %}
