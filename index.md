---
title: Page Overview
keywords: homepage
permalink: index.html
---

<nav class="sidebar-nav">
  <a class="sidebar-nav-item{% if page.url == site.baseurl %} active{% endif %}" href="{{ site.baseurl }}/">Home</a>

  {% comment %}
    The code below dynamically generates a sidebar nav of pages with
    `layout: page` in the front-matter. See readme for usage.
  {% endcomment %}

    {% assign cts = site.docs | group_by:"category" %}
      {% for ctx in cts %}
      <div class="sidebar-nav-category">
        {% if ctx.name !=''%}
          {{ ctx.name | replace: "_", " "}}
        {% else %}
          no category
        {% endif %}
      </div>
      {% for current in ctx.items %}
            <a class="sidebar-nav-item{% if page.url == current.url %} active{% endif %}" href="{{ current.url }}">{{ current.title }}</a>
      {% endfor %}
    {% endfor %}
</nav>
