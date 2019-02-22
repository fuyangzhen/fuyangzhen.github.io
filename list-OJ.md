---
layout: page
title: OJ
comments: true
---
## Online judge collection
{% for post in site.posts %}
{% if post.tags contains 'oj' %}
  * {{ post.date | date_to_string }} / [ {{ post.title }} ]({{ post.url }})
{% endif %}
{% endfor %}
