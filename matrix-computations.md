---
layout: page
title: "Matrix Computations"
permalink: /matrix-computations/
---
<ul>
  {% assign sorted_items = site.matrix-computations | sort: 'order' %}

  {% for item in sorted_items %}
    <li>
      <a href="{{ item.url }}">{{ item.title }}</a>
    </li>
  {% endfor %}
</ul>
