---
title: "Projects"
permalink: /projects/
layout: single
author_profile: true
classes: wide
---

A few things I've built that I'm proud of — each one with the problem, what I did, and what came out of it.

{% assign projects = site.projects | sort: "order" %}
{% if projects.size == 0 %}
<p><em>Write-ups coming soon.</em></p>
{% else %}
{% for pr in projects %}
{% include project-card.html project=pr heading="h2" %}
{% endfor %}
{% endif %}
