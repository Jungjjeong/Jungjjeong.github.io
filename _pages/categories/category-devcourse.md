---
layout: archive
permalink: devCourse/
title: "Dev Course"

author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.devCourse %}
{% for post in posts %} 
	{% include archive-single.html type=page.entries_layout %} 
{% endfor %}