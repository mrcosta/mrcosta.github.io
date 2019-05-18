---
layout: default
title: Book Reviews 
---
<div class="page-content wc-container">
  <ul class="posts">
      {% for post in site.posts %}
        {% if post.categories contains "books" %}
          <li>
            <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
          </li>
        {% endif %}
      {% endfor %}
  </ul>
</div>
