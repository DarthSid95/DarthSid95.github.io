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

---

## Overview

This project presents my Ph.D. thesis as an interactive series of blog posts, breaking down the mammoth 512-pager into digestible chapters.



## Abstract

> Generative adversarial networks are a popular generative modeling framework, where the task is to learn the underlying distribution of data. GANs comprise a min-max game between two neural networks, the generator and the discriminator. The generator transforms noise, typically Gaussian distributed, into a desired output, typically images. The discriminator learns to distinguish between the target samples and the generator output. The objective is to learn the optimal generator --- one that can generate samples that perfectly confuse the discriminator. GANs are trained to either minimize a divergence function or an integral probability metric (IPM) between the data and generator distributions. Common divergences include the Jensen-Shannon divergence in the standard GAN (SGAN), the chi-squared divergence in least-squares GAN (LSGAN) and f-divergences in $$f$$-GANs. Popular IPMs include the Wasserstein-2 metric or the Sobolev metric. The choice of the IPM results in a constraint class over which the discriminator is optimized, such as Lipschitz-1 functions in Wasserstein GAN (WGAN) or functions with bounded energy in their gradients as in the case of Sobolev GAN. While GANs excel at generating realistic images, their optimization is not well understood. This thesis focuses on understanding the optimality of GANs, viewed from the perspective of Variational Calculus. The thesis is organized into three parts.
>
> In Part-I, we consider the functional analysis of the discriminator in various GAN formulations. In $$f$$-GANs, the functional optimization of the loss coincides with pointwise optimization as reported in the literature. We extend the analysis to novel GAN losses via a new contrastive-learning framework called Rumi-GAN, in which the target data is split into positive and negative classes. We design novel GAN losses that allow for the generator to learn the positive class while the discriminator is trained on both classes. For the WGAN IPM, we propose a novel variant of the gradient-norm penalty, and show by means of Euler-Lagrange analysis, that the optimal discriminator solves the Poisson partial differential equation (PDE). We solve the PDE via two approaches -- one involving Fourier-series approximations and the other, involving radial basis function (RBF) expansions. We derive approximation bounds for the Fourier-series discriminator in high-dimensional spaces, and implement the discriminator by means of a novel Fourier series network architecture. The proposed approach outperforms baseline GANs on synthetic Gaussian learning tasks. We extend the approach to image generation by means of latent-space matching in Wasserstein autoencoders (WAE). The RBF formulation results in a charge-field interpretation of the optimal discriminator. We also present generalizations to higher-order gradient penalties for the LSGAN and WGAN losses, and show that the optimal discriminator can be implemented by means of a polyharmonic spline interpolator, giving rise to the name PolyGANs. PolyGANs, implemented by means of an RBF discriminator whose weights and centers are evaluated in closed-form, results in superior convergence of the generator.
>
> In Part-II, we tackle the issue of choosing the input distribution of the generator. We introduce Spider GANs, a generalization of image-to-image translation GANs, wherein providing the generator with data coming from a closely related/"friendly neighborhood" source dataset accelerates and stabilizes training, even in scenarios where there is no visual similarity between the source and target datasets. Spider GANs can be cascaded, resulting in state-of-the-art performance when trained with StyleGAN architectures on small, high-resolution datasets, in merely one-fifth of the training time. To identify "friendly neighbors" of a target dataset, we propose the "signed Inception distance" (SID), which employs the PolyGAN discriminator to quantify the proximity between datasets.
>
> In Part-III, we extend the analysis performed in Part-I to GAN generators. In divergence-minimizing GANs, the optimal generator matches the gradient of its push-forward distribution with the gradient of the data distribution (known as the score), linking GANs to score-based Langevin diffusion. In IPM-GANs, the optimal generator performs flow-matching on the gradient-field of the discriminator, thereby deriving an equivalence between the score-matching and flow-matching frameworks. We present implementations of flow-matching GANs, and develop an active-contour-based technique to train the generator in SnakeGANs. Finally, we leverage the gradient field of the discriminator to evolve particles in a Langevin-flow setting, and show that the proposed discriminator-guided Langevin diffusion accelerates baseline score-matching diffusion without the need for noise conditioning.

## Part 0 -- Introduction

{% assign thesis_posts = site.posts | where_exp: "post", "post.categories contains 'thesis-chapters-p0'" | sort: "date" %}
{% if thesis_posts.size > 0 %}
<div class="publications">
{% for post in thesis_posts %}
  <div class="row">
    <div class="col-sm-12">
      <h4><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h4>
      <p>{{ post.description }}</p>
    </div>
  </div>
{% endfor %}
</div>
{% else %}
<p><em>Rest of the thesis chapters yet to be published. Check back soon!</em></p>
{% endif %}

## Part I -- Introduction

{% assign thesis_posts = site.posts | where_exp: "post", "post.categories contains 'thesis-chapters-p1'" | sort: "date" %}
{% if thesis_posts.size > 0 %}
<div class="publications">
{% for post in thesis_posts %}
  <div class="row">
    <div class="col-sm-12">
      <h4><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h4>
      <p>{{ post.description }}</p>
    </div>
  </div>
{% endfor %}
</div>
{% else %}
<p><em>Rest of the thesis chapters yet to be published. Check back soon!</em></p>
{% endif %}


<!-- <p class="post-meta">{{ post.date | date: '%B %d, %Y' }}</p> -->