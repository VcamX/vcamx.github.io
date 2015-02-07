---
layout: page
title: Archive
---

<div>
  {% for post in site.posts %}
    {% capture currentyear %}{{ post.date | date: "%Y" }}{% endcapture %}
    {% if currentyear != year %}
      {% unless forloop.first %}
      </ul>
      {% endunless %}
      <h5>{{ currentyear }}</h5>
      <ul>
      {% capture year %}{{ currentyear }}{% endcapture %} 
    {% endif %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
  {% if site.posts %}
  </ul>
  {% endif %}
</div>