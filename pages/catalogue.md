---
layout: catalogue
title: Catalogue
permalink: /catalogue/
---

<div class="grid">
  {% assign sorted_posts = site.posts | sort: 'date' | reverse %}
  {% for post in sorted_posts %}
    {% unless post.catalogue == false %}
      {% assign url_parts = post.url | split: "/" %}
      {% assign folder_name = url_parts | last %}
      {% if folder_name == "" %}
        {% assign folder_name = url_parts[ url_parts.size - 2 ] %}
      {% endif %}
      <div class="card">
        <a href="{{ post.url }}">
          <div class="card-image" style="background-image: url('/assets/images/{{ folder_name }}/cover.jpg');"></div>
          <div class="card-content">
            <div class="card-header">
              <h2>{{ post.title }}</h2>
              <span class="post-date">{{ post.date | date: "%b %d, %Y" }}</span>
            </div>
            <p>{{ post.subtitle }}</p>
          </div>
        </a>
      </div>
    {% endunless %}
  {% endfor %}
</div>
