---
layout: page
title: D&A
comments: true
---
## Data structure & algorithm
{% for post in site.posts %}
{% if post.tags contains 'dna' %}
  * {{ post.date | date_to_string }} / [ {{ post.title }} ]({{ post.url }})
{% endif %}
{% endfor %}
