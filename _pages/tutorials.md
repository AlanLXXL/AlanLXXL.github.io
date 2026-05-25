---
title: "Tutorials"
permalink: /tutorials/
layout: single
author_profile: true
classes: wide
---

Practical tutorials for software and tools that don't always make it into a Computer Science curriculum. Each one is something I've actually used — written for fellow students who want a faster path from "I've heard of this" to "I'm using it confidently."

{% assign all_series = site.series | sort: "order" %}
{% if all_series.size > 0 %}
## Series

{% for s in all_series %}
{% assign part_count = site.tutorials | where: "series", s.slug | size %}
<article class="series-card">
  <div class="series-card__eyebrow">Series &middot; {{ part_count }} part{% if part_count != 1 %}s{% endif %}</div>
  <h3 class="series-card__title">
    <a href="{{ s.url | relative_url }}">{{ s.title }}</a>
  </h3>
  {% if s.excerpt %}
  <p class="series-card__excerpt">{{ s.excerpt | markdownify | strip_html | truncate: 240 }}</p>
  {% endif %}
  <p class="series-card__meta">
    {% if s.audience %}<strong>For:</strong> {{ s.audience }}{% endif %}
    {% if s.status %} &middot; <em>{{ s.status }}</em>{% endif %}
  </p>
</article>
{% endfor %}

## Standalone tutorials
{% endif %}

<div class="grid__wrapper">
{% assign standalone = site.tutorials | where_exp: "t", "t.series == nil" %}
{% assign sorted_tutorials = standalone | sort: 'last_tested' | reverse %}
{% for tutorial in sorted_tutorials %}
  <div class="grid__item">
    <article class="archive__item" itemscope itemtype="https://schema.org/CreativeWork">
      <h2 class="archive__item-title" itemprop="headline">
        <a href="{{ tutorial.url | relative_url }}" rel="permalink">{{ tutorial.title }}</a>
      </h2>
      <div class="archive__item-excerpt" itemprop="description">
        {% if tutorial.app %}<strong>{{ tutorial.app }}</strong>{% endif %}
        {% if tutorial.difficulty %} &middot; {{ tutorial.difficulty }}{% endif %}
        {% if tutorial.time %} &middot; ~{{ tutorial.time }}{% endif %}
      </div>
      <p class="archive__item-excerpt" itemprop="description">{{ tutorial.excerpt | markdownify | strip_html | truncate: 180 }}</p>
      {% if tutorial.last_tested %}
      <p class="archive__item-excerpt"><small><em>Last tested: {{ tutorial.last_tested | date: "%B %Y" }}</em></small></p>
      {% endif %}
    </article>
  </div>
{% endfor %}
</div>
