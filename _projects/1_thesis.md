---
layout: page
title: "On the Optimality of Generative Adversarial Networks — A Variational Perspective"
description: "A chapter-by-chapter exploration of my research on GANs"
img: assets/img/thesis/thesis-cover.png
importance: 1
category: work
related_publications: true
---

<div class="video-banner" style="display: flex; width: 100%; margin: 0; padding: 0; overflow: hidden;">
  <video style="width: 33.333%; height: auto; object-fit: cover; margin: 0; padding: 0; display: block;" autoplay loop muted playsinline>
    <source src="{{ '/assets/video/FFHQ.mp4' | relative_url }}" type="video/mp4">
  </video>
  <video style="width: 33.333%; height: auto; object-fit: cover; margin: 0; padding: 0; display: block;" autoplay loop muted playsinline>
    <source src="{{ '/assets/video/UkiFaces.mp4' | relative_url }}" type="video/mp4">
  </video>
  <video style="width: 33.334%; height: auto; object-fit: cover; margin: 0; padding: 0; display: block;" autoplay loop muted playsinline>
    <source src="{{ '/assets/video/MetFaces.mp4' | relative_url }}" type="video/mp4">
  </video>
</div>

## Overview

This project presents my Ph.D. thesis as an interactive series of blog posts, breaking down the mammoth 512-pager into digestible chapters.

## Abstract

{% assign thesis_posts = site.posts | where_exp: "post", "post.categories contains 'thesis-chapters'" | sort: "date" %}
{% if thesis_posts.size > 0 %}
<div class="publications">
{% for post in thesis_posts %}
  <div class="row">
    <div class="col-sm-12">
      <h4><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h4>
      <p>{{ post.description }}</p>
      <p class="post-meta">{{ post.date | date: '%B %d, %Y' }}</p>
    </div>
  </div>
{% endfor %}
</div>
{% else %}
<p><em>No thesis chapters published yet. Check back soon!</em></p>
{% endif %}
-->