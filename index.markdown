---
title: Home
layout: list
---


Welcome to my personal portfolio. Here, you will find a curated collection of my creative endeavors and professional achievements. 

## Pages


{% for page in site.pages %}
{% if page.layout == "page" and page.collection != "do_not_list" %}
- [{{ page.title }}]({{ page.permalink }})
{% endif %}
{% endfor %}
