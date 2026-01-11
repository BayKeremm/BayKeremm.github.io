---
layout: page
title: "Computational Geometry"
permalink: /computational-geometry/
---
<div class="compact-list">
  {% assign sorted_items = site.computational-geometry | sort: 'order' %}
  {% for item in sorted_items %}
    <a href="{{ item.url }}" class="compact-item">
      
      <div class="compact-content">
        <span class="compact-title">{{ item.title }}</span>
        {% if item.description %}
          <span class="compact-desc">— {{ item.description }}</span>
        {% endif %}
      </div>
      
      <span class="compact-arrow">&rarr;</span>
    </a>
  {% endfor %}
</div>
