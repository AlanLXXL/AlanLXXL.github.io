---
layout: single
title: "About"
author_profile: true
---

{% include about-content.md %}

{% assign featured = site.projects | sort: "order" %}
{% if featured.size > 0 %}
<section class="home-projects">
  <h2 id="selected-projects">Selected projects</h2>
  {% for pr in featured limit: 3 %}
  {% include project-card.html project=pr heading="h3" %}
  {% endfor %}
  {% if featured.size > 3 %}
  <p><a href="{{ '/projects/' | relative_url }}">See all projects &rarr;</a></p>
  {% endif %}
</section>
{% endif %}
