---
layout: catalogue
title: Catalogue
permalink: /catalogue/
---

<div class="grid">
  <!-- =========================================================================
       БЛОК ИНИЦИАЛИЗАЦИИ: Получаем и сортируем все посты
       ========================================================================= -->
  {% assign sorted_posts = site.posts | sort: 'date' | reverse %}
  {% for post in sorted_posts %}
    {% unless post.catalogue == false %}
      
      <!-- =====================================================================
           БЛОК ПУТЕЙ: Определяем имя папки на основе URL поста
           ===================================================================== -->
      {% assign url_parts = post.url | split: "/" %}
      {% assign folder_name = url_parts | last %}
      {% if folder_name == "" %}
        {% assign folder_name = url_parts[ url_parts.size - 2 ] %}
      {% endif %}
      
      <div class="card">
        <a href="{{ post.url }}">
          
          <!-- =================================================================
               БЛОК ПРОВЕРКИ ВИДЕО: Ищем cover.webm в статических файлах сайта,
               чтобы избежать ошибок 404 в браузере при отсутствии файла
               ================================================================= -->
          {% assign webm_path = '/assets/images/' | append: folder_name | append: '/cover.webm' %}
          {% assign has_webm = false %}
          {% for static_file in site.static_files %}
            {% if static_file.path == webm_path %}
              {% assign has_webm = true %}
              {% break %}
            {% endif %}
          {% endfor %}

          <!-- =================================================================
               БЛОК ОТРИСОВКИ ОБЛОЖКИ: Видео-превью (если есть) или JPG
               Атрибуты: autoplay, loop, muted, playsinline делают из видео "gif"
               ================================================================= -->
          {% if has_webm %}
            <video class="card-video" src="{{ webm_path }}" autoplay loop muted playsinline disablePictureInPicture></video>
          {% else %}
            <div class="card-image" style="background-image: url('/assets/images/{{ folder_name }}/cover.jpg');"></div>
          {% endif %}
          
          <!-- =================================================================
               БЛОК КОНТЕНТА: Заголовок, дата и подзаголовок
               ================================================================= -->
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