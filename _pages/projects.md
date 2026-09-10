---
title: "Projects"
permalink: /projects/
layout: single
author_profile: true
classes: wide
---

Three projects, three angles on how I build: shipping a full-stack app end to end, running a research project with controlled experiments, and leading part of a seven-person C++ team. Each write-up covers the problem, the design decisions, the results with numbers, and what I would do differently.

{% assign projects = site.projects | sort: "order" %}
{% if projects.size == 0 %}
<p><em>Write-ups coming soon.</em></p>
{% else %}
{% for pr in projects %}
{% include project-card.html project=pr heading="h2" %}
{% endfor %}
{% endif %}
