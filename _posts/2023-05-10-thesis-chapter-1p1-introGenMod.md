---
layout: post
title: "Thesis Chapter 1.1: Introduction to Generative Modeling"
date: 2023-05-10 22:00:00
description: "An introduction to generative modeling, covering autoencoders, VAEs, GANs, flows, and diffusion models"
tags: thesis GANs machine-learning generative-models
categories: thesis-chapters
related\_posts: true
featured: true
toc:
  beginning: true
---

> *What I cannot create, I do not understand.*  
> — Richard P. Feynman

## Generative Models

Generative modeling is a branch of machine learning that addresses the problem of learning the distribution of given data samples, which can subsequently be used to create more such samples. Generative models have gained immense popularity in the generation of images, text, and audio and learning multi-modal transformations. In this thesis, we focus on image datasets, and consider generative modelling for images. Consider the dataset $$\mathcal{D} = \big\{\mathbf{x}\_i~\|~\mathbf{x}\_i\in\mathbb{R}^n,\,i=1,2,\ldots,N\big\}$$, with samples drawn from an unknown distribution $$p\_d$$ (the *data distribution*). We are considering $$\mathbf{x}\_i$$ to be the vectorized representation of an image, with $$n = h\times w\times c$$, where $$(h,w,c)$$ denote the height, width, and the number of channels in the image. This representation also allows one to consider multichannel images, such as color images, multispectral images, and hyperspectral images.

Neural-network-based generative models output samples $$\tilde{\mathbf{x}}$$ that are governed by a parametric distribution $$p\_{\text{model}}(\tilde{\mathbf{x}};\theta)$$. The goal in optimizing/training the neural network model is to learn the optimal parameters $$\theta^\*$$ such that the generated samples appear realistic, and $$p\_{\text{model}}(\tilde{\mathbf{x}};\theta^\*) = p\_d$$. Early neural-network-based generative models, going back to the 1980s, are energy-based models (EBMs) that define an energy functional on data samples. However, these models relied on Markov chain Monte Carlo (MCMC) for sampling, and could not be scaled to high-dimensional objects such as images.

Modern approaches to generative modelling rely on the *Manifold Hypothesis*, which posits that although the ambient dimension of images is large $$(\mathbf{x}\in\mathbb{R}^n)$$, they actually reside in a much lower-dimensional manifold $$(\mathbf{z}\in\mathbb{R}^d)$$ with $$d\ll n$$, often several orders lower. Leveraging this property, generative models have targeted nearly invertible low-dimensional *latent* representations $$\{\mathbf{z}\_i\}$$ of images $$\{\mathbf{x}\_i\}$$. Autoencoders (AEs), variational autoencoders (VAEs), Wasserstein autoencoders (WAEs), generative adversarial networks (GANs), and variants thereof, are all examples of such generative models. By contrast, advanced frameworks such as normalizing flows (NFs) and diffusion models (DMs) operate on the ambient dimensionality of the data and are far more challenging to train and intensive in terms of memory and compute. Our focus in this thesis is predominantly on GANs, with occasional connections to WAEs, NFs, and DMs.

We briefly introduce the various generative models considered in this thesis.

### Autoencoders

Autoencoders comprise a pair of neural networks, namely the encoder $$(\text{Enc}\_{\phi})$$ and decoder $$(\text{Dec}\_{\theta})$$, trained to learn a compressed (low-dimensional) representation of the data $$\mathcal{D}$$. Given a sample $$\mathbf{x}\_i$$, the encoder learns a latent representation of the data $$\mathbf{z}\_i = \text{Enc}\_{\phi}(\mathbf{x}\_i)$$, while the decoder learns to invert the encoder transformation, $$\tilde{\mathbf{x}}\_i = \text{Dec}\_{\theta}(\mathbf{z}\_i)$$ such that $$\tilde{\mathbf{x}}\_i \approx \mathbf{x}\_i$$. While the encoder and decoder networks are typically fully-connected multi-layer perceptron (MLP) networks, convolutional AEs (CAEs) consider encoders with convolutional layers, while the decoders consist of transposed convolution layers. While autoencoders were originally proposed as the nonlinear counterpart of principal component analysis (PCA), in the recent literature, AEs have been leveraged to implement flow-based and generative pre-training architectures.

The standard AE optimization involves training the encoder-decoder pair to generate accurate reconstructions. Given the dataset $$\mathcal{D}$$, the $$N$$-sample estimate of the AE loss is given by

$$
\mathcal{L}^{\text{AE}}(\phi,\theta) = \frac{1}{N}\sum\_{i=1,\,\mathbf{x}\_i\sim\mathcal{D}}^{N}\text{dist}\left(\mathbf{x}\_i,\tilde{\mathbf{x}}\_i \right) = \frac{1}{N}\sum\_{i=1,\,\mathbf{x}\_i\sim\mathcal{D}}^{N}\text{dist}\left(\mathbf{x}\_i,\text{Dec}\_{\theta}\left( \text{Enc}\_{\phi}\left(\mathbf{x}\_i \right) \right) \right),
$$

where in turn, $$\phi^\*$$ and $$\theta^\*$$ denote the optimal encoder and decoder parameters:

$$
(\phi^\*,\theta^\*) = \arg\min\_{\theta,\phi} \left\{ \mathcal{L}^{\text{AE}} \right\}.
$$

Typical choices for the distance measure include the $$\ell\_1$$ or $$\ell\_2$$ loss:

$$
\mathcal{L}\_{\text{L}\_p}^{\text{AE}}(\phi,\theta) = \frac{1}{N}\sum\_{i=1,\,\mathbf{x}\_i\sim\mathcal{D}}^{N}\\|\mathbf{x}\_i - \text{Dec}\_{\theta}\left( \text{Enc}\_{\phi}\left(\mathbf{x}\_i \right) \right) \\|\_{\ell\_p},
$$

where $$p = 1$$ or $$2$$, respectively, or a weighted combination of these. Regularized autoencoders have also been considered, wherein the encoder and decoder networks are regularized to either be smooth transformations or learn sparse representations. Although autoencoders are generative models in the sense that the decoders learn to output realistic images given a latent representation, a major limitation is their inability to generate new/unseen images. This can be attributed to the lack of a closed-form or parametric structure to the latent-space distribution, which results in inferior reconstructions of latent-space interpolations. The variational and Wasserstein autoencoder schemes alleviate this issue, by enforcing constraints on the distribution of the latent representations.

### Variational Autoencoders

While AEs consider the encoder output as a *deterministic* representation of the input image, in variational autoencoders (VAEs), the latent representations are assumed to be random, with the objective of modelling the joint distribution $$p\_{\text{model}}(\mathbf{x},\mathbf{z})$$, from which the data distribution can be computed as $$p\_d(\mathbf{x}) = \int\_{\mathcal{Z}} p\_{\text{model}}(\mathbf{x},\mathbf{z})\,d\mathbf{z}$$, where $$\mathcal{Z}$$ denotes the support of the latent-space distribution. Invoking Bayes' rule, we have $$p\_{\text{model}}(\mathbf{x},\mathbf{z}) = \frac{p\_{\text{model}}(\mathbf{x}\|\mathbf{z}) p\_z(\mathbf{z})}{p\_{\text{model}}(\mathbf{z}\|\mathbf{x})}$$. The decoder can be interpreted as approximating the posterior, *i.e.*, $$\text{Dec}\_{\theta}(\mathbf{z}) \sim p\_{\text{model}}(\mathbf{x}\|\mathbf{z})$$. As the underlying data distribution is inaccessible, the encoder models an approximation of the likelihood, $$\text{Enc}\_{\phi}(\mathbf{x}) \sim q(\mathbf{z}\|\mathbf{x}) \approx p\_{\text{model}}(\mathbf{z}\|\mathbf{x})$$. In VAEs, one optimizes the so called evidence lower bound (ELBO), given by

$$
\mathcal{L}\_{\text{ELBO}}^{\text{VAE}} = \underbrace{\mathbb{E}\_{\mathbf{z}\sim \text{Enc}\_{\phi}(\mathbf{x})} \left[ \ln\left(p\_{\text{model}}(\mathbf{x}\|\mathbf{z})\right)\right]}\_{\text{reconstruction error}} - \underbrace{D\_{\text{KL}}\left( q(\mathbf{z}\|\mathbf{x}) \,\\|\, p\_z(\mathbf{z})\right)}\_{\text{prior matching error}},
$$

where $$D\_{\text{KL}}$$ denotes the Kullback-Leibler (KL) divergence between two distributions. The reconstruction error, as in the case of AEs, can be approximated in terms of the $$\ell\_2$$ or $$\ell\_1$$ loss. The KL divergence can be further simplified by assuming a Gaussian model $$p\_z(\mathbf{z})$$, with the latent sample defined by means of a *change-of-variable* formula as $$\mathbf{z}\_i = \mu\_{\phi}(\mathbf{x}\_i)+ \sigma\_{\phi}(\mathbf{x}\_i)\odot\boldsymbol{\epsilon}$$, where $$(\mu\_{\phi},\sigma\_{\phi})= \text{Enc}\_{\phi}(\mathbf{x}\_i)$$ and $$\boldsymbol{\epsilon}$$ is drawn from the standard normal distribution, *i.e.*, $$\boldsymbol{\epsilon}\sim\mathcal{N}(\mathbf{0}\_d,\mathbb{I}\_d)$$, where in turn, $$\mathbf{0}\_d$$ denotes the $$d$$-dimensional null vector, $$\mathbb{I}\_d$$ is the $$d$$-dimensional identity matrix, and $$\odot$$ stands for pointwise multiplication. Although VAEs are superior to AEs in terms of enforcing structure on the latent space, generating novel unseen samples remains challenging. Further, the use of the $$\ell\_2$$ cost results in over-smoothing of the reconstructed images, owing to the implicit Gaussian prior that this choice of loss introduces. Generative adversarial networks (GANs) were proposed as an alternative to VAEs, to alleviate its limitations.

### Navigation

- **Next:** [Chapter 1.2: Generative Adversarial Networks](/blog/2023/thesis-chapter-1p2-GANs/)
- **Back to:** [Thesis Project](/projects/1\_thesis/)


