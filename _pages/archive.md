---
title: Archive
layout: archive
permalink: /archive/
author_profile: false
description: "Browse the full archive of blog posts from Chronicles of Rishi, covering embedded systems, TinyML, STM32, ESP32, and machine learning."
---

All posts in one place.

{% assign postsByYear = site.posts | group_by_exp: 'post', 'post.date | date: "%Y"' %}
{% for year in postsByYear %}
  <h2 id="{{ year.name }}">{{ year.name }}</h2>
  <ul>
    {% for post in year.items %}
      <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        <small>({{ post.date | date: "%B %d" }})</small>
      </li>
    {% endfor %}
  </ul>
{% endfor %}
