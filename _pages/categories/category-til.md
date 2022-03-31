---
layout: archive
permalink: til/
title: "TIL"

author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.TIL %}
{% for post in posts %} 
	{% include archive-single.html type=page.entries_layout %} 
{% endfor %}