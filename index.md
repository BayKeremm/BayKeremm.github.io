---
layout: page
---

<div class="collection-stack">
  {% for collection in site.collections %}
    {% unless collection.label == 'posts' %}
      
      <a href="/{{ collection.label }}/" class="stack-row">
        <div class="row-content">
          <h3 class="row-title">{{ collection.label | capitalize }}</h3>
        </div>
        <div class="row-meta">
          <span class="row-arrow">&rarr;</span>
        </div>
      </a>

    {% endunless %}
  {% endfor %}
</div>
