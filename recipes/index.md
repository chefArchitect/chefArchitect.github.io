---
layout: page
title: Recipes
excerpt: "Recipes straight from the chef"
search_omit: true
---

{% for post in site.categories.recipes reversed %}
<div class="media">
      <div class="media-body">
        <a href="{{ site.url }}{{ post.url }}">
          <h4 class="media-heading">{{ post.title }}</h4>
        </a>
        {{ post.excerpt }}
      </div>
      <div class="media-right">
        <a href="{{ site.url }}{{ post.url }}">
          <img class="media-object" src="{{ site.url }}/images/{{ post.image.thumb}}" style="width: 64px; height: 64px;">
        </a>
      </div>
    </div>
{% endfor %}
