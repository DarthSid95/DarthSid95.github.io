---
layout: post
title: "Thesis Chapter 1.3: Flow-based Models and Diffusion"
date: 2023-05-10 02:00:00
description: "An introduction to normalizing flows and score-based diffusion models"
tags: thesis flows diffusion machine-learning generative-models
categories: thesis-chapters
related_posts: true
featured: true
toc:
  beginning: true
---

> *What I cannot create, I do not understand.*  
> — Richard P. Feynman

## Flow-based Models

Flow-based models such as normalizing flows (NFs) leverage the *change-of-variables* formula to learn a transformation from a parametric prior distribution to the target. Unlike in GANs and AEs, in NF models, the input to model $$\mathbf{z}$$ is assumed to be of the same dimensionality as the output, *i.e.*, $$\mathbf{z}\in\mathbb{R}^{n}$$. Normalizing flows model a forward process as the gradual *corruption* of a target distribution $$p_d$$ to the noise distribution $$p_z$$. The forward process is modeled as a series of *invertible* transformations $$f_i$$, such that the composite function yields the desired *push-forward* distribution:

$$
\mathbf{z} = \left( f_N \circ f_{N-1} \circ \cdots \circ f_2 \circ f_1\right) (\mathbf{x}) = f(\mathbf{x});~\mathbf{x}\sim p_d.
$$

Then, by the *change-of-variables* formula, we have

$$
p_z\left(\mathbf{z}\right) = p_d\left(f^{-1}(\mathbf{z})\right) \left\vert \mathrm{det}\, \mathbf{J}_{f^{-1}}(\mathbf{z})\right\vert = p_d\left(f^{-1}(\mathbf{z})\right) \left\vert \mathrm{det}\, \mathbf{J}_{f}\left(f^{-1}(\mathbf{z})\right)\right\vert^{-1},
$$

where $$\left\vert \mathrm{det}\, \mathbf{J}_{f}(\cdot)\right\vert$$ denotes the determinant of the Jacobian of $$f$$. Consider a vector $$\mathbf{x} = [x_1, x_2,\,\ldots\,,x_n]^{\mathsf{T}} \in \mathbb{R}^n$$ and the function $$f:\mathbb{R}^n \rightarrow \mathbb{R}^n$$, *i.e.*, $$f(\mathbf{x}) = [f^1(\mathbf{x}), f^2(\mathbf{x}),\ldots,f^n(\mathbf{x})]$$. The notation $$\nabla_{\mathbf{x}} f(\mathbf{x})$$ represents the gradient matrix associated with the generator, with entries consisting of the partial derivatives of the entries of $$f$$ with respect to the entries of $$\mathbf{x}$$:

$$
\nabla_{\mathbf{x}} f(\mathbf{x}) = \left[\begin{matrix}
\frac{\partial f^1}{\partial x_1} & \frac{\partial f^2}{\partial x_1} & \ldots & \ldots &\frac{\partial f^n}{\partial x_1} \\[3pt]
\frac{\partial f^1}{\partial x_2} & \frac{\partial f^2}{\partial x_2} & \ldots & \ldots &\frac{\partial f^n}{\partial x_2} \\[3pt]
\vdots&\vdots&\ddots&\ddots&\vdots\\
\frac{\partial f^1}{\partial x_n} & \frac{\partial f^2}{\partial x_n} & \ldots & \ldots &\frac{\partial f^n}{\partial x_n} 
\end{matrix} \right].
$$

The Jacobian $$\mathbf{J}$$ can be thought of as *measuring* the transformation that the function imposes locally at the point of evaluation, and is defined to be the transpose of the gradient, *i.e.,* $$\nabla_{\mathbf{z}}f(\mathbf{z}) = \mathbf{J}_f^{\mathsf{T}}(\mathbf{z})$$. The generative model is then defined as the *reverse* process, given by:

$$
p_d(\mathbf{x}) =  p_z\left(f^{-1}(\mathbf{x})\right) \left\vert \mathrm{det}\, \mathbf{J}_{f}(\mathbf{x})\right\vert = p_z\left(f^{-1}(\mathbf{x})\right) \prod_{i=1}^{N} \left\vert \mathrm{det}\, \mathbf{J}_{f_i}(\mathbf{z}_{i-1})\right\vert,
$$

where $$\mathbf{z}_i = f_i(\mathbf{z}_{i-1})$$ with $$\mathbf{z}_0 = \mathbf{z}$$ and $$\mathbf{z}_N = \mathbf{x}$$. An in-depth analysis of normalizing flows is presented by recent comprehensive surveys. The $$f_i$$ network architecture is constrained to facilitate easy computation of the Jacobian. Recent approaches design flows based on autoregressive models, or architectures motivated by the Sobolev GAN loss. Flows have also been explored in the context of improving the quality of the noise input in GANs.

## Score-based Diffusion Models

Diffusion models are a recent class of generative models that implement discretized Langevin flow, and can be viewed as a random walk in a high-dimensional space. The random walks correspond to a Markov chain, whose stationary distribution converges to the desired distribution. As in the case of flow-based approaches, a forward process is assumed, wherein the data is *corrupted* with noise, while the generative model is an approximation of the reverse process. Langevin Monte Carlo (LMC) is the canonical algorithm for sampling from a given distribution in this framework, where we have access to the *score function*, $$\nabla_{\mathbf{x}}\ln p_d(\mathbf{x})$$, which is the gradient of the logarithm of the target distribution. LMC is an instance of Markov chain Monte Carlo (MCMC), and is a discretization of the Langevin diffusion stochastic differential equation given by

$$
d\mathbf{x}(t) = -\nabla_{\mathbf{x}}f(\mathbf{x}(t))\,dt + \sqrt{2}\,d\mathbf{B}(t),
$$

where $$\mathbf{B}(t)$$ is the standard Brownian motion. The associated discretized update is given by

$$
\mathbf{x}_{t+1} = \mathbf{x}_{t} - \epsilon \left. \nabla_{\mathbf{x}}f(\mathbf{x}) \right|_{\mathbf{x}=\mathbf{x}_t} + \sqrt{2\epsilon}\,\mathbf{z}_t,
$$

where $$\mathbf{z}_{t}$$ is an instance of the noise distribution drawn at time instant $$t$$ and is independent of the sample $$\mathbf{x}_{t}$$ generated at the corresponding time instant. As in normalizing flows, we have $$\mathbf{z}\in\mathbb{R}^{n}$$. The evolution is initialized with parametric noise, *i.e.*, $$\mathbf{x}_0 \sim p_z$$. The choice of the mapping function $$f$$ gives rise to various diffusion models. For example, the popular family of score-based models consider $$f(\mathbf{x}) = \nabla_{\mathbf{x}}\ln p_d(\mathbf{x})$$, or in inverse-heat diffusion models, we have a frequency-domain transformation corresponding to Gaussian deblurring. In this thesis, we primarily focus on score-based diffusion, and their relation to GAN optimization.

Existing score-based approaches train a neural network $$s_{\theta}(\mathbf{x})$$ to approximate the score, by means of a score-matching loss, originally considered in the context of independent component analysis:

$$
\mathcal{L}(\theta)=\frac{1}{2} \,\mathbb{E}_{{\mathbf{x}}\sim p_d} \left[ \left\Vert s_{\theta}(\mathbf{x}) - \nabla_{\mathbf{x}}\ln p_d(\mathbf{x}) \right\Vert \right\Vert_2^2 \right].
$$

The output of the trained network is used to generate samples through the annealed Langevin dynamics in noise-conditioned score networks (NCSN). However, a major limitation is that the predicted score is *weak* in regions far away from the target distribution, which is typically the case at the start of the Markov chain, around $$\mathbf{x}_0\sim p_z$$. Various approaches such as noise scaling, sliced SM, and denoising SM have been proposed to improve the strength of the gradients. Works considering improved discretization of the underlying differential equation have been developed to accelerate the sampling process. Recently, denoising diffusion GANs (DDGANs) were introduced, wherein a GAN is trained to model the diffusion process, with the generator and discriminator networks conditioned on the time index.

### Navigation

- **Previous:** [Chapter 1.2: Generative Adversarial Networks](/thesis-chapters/2023/05/10/thesis-chapter-1p2-IntroGANs.html)
- **Next:** [Chapter 1.4: A Deep Dive into GANs](/blog/2023/thesis-chapter-1p3-VarCalc/)
- **Back to:** [Thesis Project](/projects/1_thesis/)
