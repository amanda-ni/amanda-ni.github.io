---
layout: page-header
title: Amanda's Coding Page
subtitle: Musings and Otherwise
css: "/css/index.css"
meta-title: "Amanda's adventures in AI, a self-driving car enthusiast"
meta-description: "I'm taking the Car Nano Degree, and am excited about machine learning!"
bigimg:
  - "/images/poster/puppachino.jpg" : "Rosie after licking a Puppacino from Starbucks"
  - "/images/poster/49ers.jpg" : "At a 49ers game, courtesy Wade"
  - "images/poster/south-france.jpg" : "In the South of France for vacation, 2016"
  - "images/poster/wisperia-trees.jpg" : "Wisperia Trees in Japan"
---

<div class="posts">
  {% for post in site.posts %}
    <article class="post">

      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>

      <div class="entry">
        {{ post.excerpt }}
      </div>

      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
    </article>
  {% endfor %}
</div>


