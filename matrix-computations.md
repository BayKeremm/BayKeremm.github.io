---
layout: page
title: "Matrix Computations"
permalink: /matrix-computations/
---

<ul>
  {% for item in site.matrix-computations %}
    <li>
      <a href="{{ item.url }}">{{ item.title }}</a>
      <p>{{ item.excerpt }}</p>
    </li>
  {% endfor %}
</ul>