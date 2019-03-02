---
layout: page
title: Design Patterns
comments: true
---
## Design Patterns Library
{% for post in site.posts %}
{% if post.tags contains 'Design Patterns' %}
  * {{ post.date | date_to_string }} / [ {{ post.title }} ]({{ post.url }})
{% endif %}
{% endfor %}
