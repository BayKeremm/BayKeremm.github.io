---
layout: page
title: "Computational Geometry"
permalink: /computational-geometry/
---
<ul>
  {% assign sorted_items = site.computational-geometry | sort: 'order' %}

  {% for item in sorted_items %}
    <li>
      <a href="{{ item.url }}">{{ item.title }}</a>
    </li>
  {% endfor %}
</ul>
