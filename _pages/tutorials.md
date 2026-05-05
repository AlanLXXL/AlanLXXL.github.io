---
title: "Tutorials"
permalink: /tutorials/
layout: single
author_profile: true
classes: wide
---

Practical tutorials for software and tools that don't always make it into a Computer Science curriculum. Each one is something I've actually used — written for fellow students who want a faster path from "I've heard of this" to "I'm using it confidently."

<div class="grid__wrapper">
{% assign sorted_tutorials = site.tutorials | sort: 'last_tested' | reverse %}
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
