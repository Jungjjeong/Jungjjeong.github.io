---
layout: archive
permalink: categories/web
title: "Web"

author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.Web %}
{% for post in posts %} 
	{% include archive-single.html type=page.entries_layout %} 
{% endfor %}