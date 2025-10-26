---
layout: post
title: "Thesis Chapter 1.1: Introduction to Generative Modeling"
date: 2023-05-10 00:00:00
description: "An introduction to generative modeling, covering autoencoders, VAEs, GANs, flows, and diffusion models"
tags: thesis GANs machine-learning generative-models
categories: thesis-chapters-p0
related_posts: true
featured: true
toc:
  beginning: true
---

> *What I cannot create, I do not understand.*  
> — Richard P. Feynman

## Generative Models

Generative modeling is a branch of machine learning that addresses the problem of learning the distribution of given data samples, which can subsequently be used to create more such samples. Generative models have gained immense popularity in the generation of images [1], text [2], and audio [3] and learning multi-modal transformations [4]. In this thesis, we focus on image datasets, and consider generative modelling for images. Consider the dataset $$\mathcal{D} = \big\{\mathbf{x}_i~\|~\mathbf{x}_i\in\mathbb{R}^n,\,i=1,2,\ldots,N\big\}$$, with samples drawn from an unknown distribution $$p_d$$ (the *data distribution*). We are considering $$\mathbf{x}_i$$ to be the vectorized representation of an image, with $$n = h\times w\times c$$, where $$(h,w,c)$$ denote the height, width, and the number of channels in the image. This representation also allows one to consider multichannel images, such as color images, multispectral images, and hyperspectral images.

Neural-network-based generative models output samples $$\tilde{\mathbf{x}}$$ that are governed by a parametric distribution $$p_{\text{model}}(\tilde{\mathbf{x}};\theta)$$. The goal in optimizing/training the neural network model is to learn the optimal parameters $$\theta^*$$ such that the generated samples appear realistic, and $$p_{\text{model}}(\tilde{\mathbf{x}};\theta^*) = p_d$$. Early neural-network-based generative models, going back to the 1980s, are energy-based models (EBMs) that define an energy functional on data samples. However, these models relied on Markov chain Monte Carlo (MCMC) for sampling [5], and could not be scaled to high-dimensional objects such as images.

Modern approaches to generative modelling rely on the *Manifold Hypothesis* [6], which posits that although the ambient dimension of images is large $$(\mathbf{x}\in\mathbb{R}^n)$$, they actually reside in a much lower-dimensional manifold $$(\mathbf{z}\in\mathbb{R}^d)$$ with $$d\ll n$$, often several orders lower. Leveraging this property, generative models have targeted nearly invertible low-dimensional *latent* representations $$\{\mathbf{z}_i\}$$ of images $$\{\mathbf{x}_i\}$$. Autoencoders (AEs) [7], variational autoencoders (VAEs) [8], Wasserstein autoencoders (WAEs) [9], generative adversarial networks (GANs) [10], and variants thereof, are all examples of such generative models. By contrast, advanced frameworks such as normalizing flows (NFs) [11] and diffusion models (DMs) [13] operate on the ambient dimensionality of the data and are far more challenging to train and intensive in terms of memory and compute. Our focus in this thesis is predominantly on GANs, with occasional connections to WAEs, NFs, and DMs.

We briefly introduce the various generative models considered in this thesis.

### Autoencoders

Autoencoders comprise a pair of neural networks, namely the encoder $$(\text{Enc}_{\phi})$$ and decoder $$(\text{Dec}_{\theta})$$, trained to learn a compressed (low-dimensional) representation of the data $$\mathcal{D}$$. Given a sample $$\mathbf{x}_i$$, the encoder learns a latent representation of the data $$\mathbf{z}_i = \text{Enc}_{\phi}(\mathbf{x}_i)$$, while the decoder learns to invert the encoder transformation, $$\tilde{\mathbf{x}}_i = \text{Dec}_{\theta}(\mathbf{z}_i)$$ such that $$\tilde{\mathbf{x}}_i \approx \mathbf{x}_i$$. While the encoder and decoder networks are typically fully-connected multi-layer perceptron (MLP) networks, convolutional AEs (CAEs) consider encoders with convolutional layers, while the decoders consist of transposed convolution layers. While autoencoders were originally proposed as the nonlinear counterpart of principal component analysis (PCA), in the recent literature, AEs have been leveraged to implement flow-based and generative pre-training architectures.

The standard AE optimization involves training the encoder-decoder pair to generate accurate reconstructions. Given the dataset $$\mathcal{D}$$, the $$N$$-sample estimate of the AE loss is given by

$$
\mathcal{L}^{\text{AE}}(\phi,\theta) = \frac{1}{N}\sum_{i=1,\,\mathbf{x}_i\sim\mathcal{D}}^{N}\text{dist}\left(\mathbf{x}_i,\tilde{\mathbf{x}}_i \right) = \frac{1}{N}\sum_{i=1,\,\mathbf{x}_i\sim\mathcal{D}}^{N}\text{dist}\left(\mathbf{x}_i,\text{Dec}_{\theta}\left( \text{Enc}_{\phi}\left(\mathbf{x}_i \right) \right) \right),
$$

where in turn, $$\phi^*$$ and $$\theta^*$$ denote the optimal encoder and decoder parameters:

$$
(\phi^*,\theta^*) = \arg\min_{\theta,\phi} \left\{ \mathcal{L}^{\text{AE}} \right\}.
$$

Typical choices for the distance measure include the $$\ell_1$$ or $$\ell_2$$ loss:

$$
\mathcal{L}_{\text{L}_p}^{\text{AE}}(\phi,\theta) = \frac{1}{N}\sum_{i=1,\,\mathbf{x}_i\sim\mathcal{D}}^{N}\\|\mathbf{x}_i - \text{Dec}_{\theta}\left( \text{Enc}_{\phi}\left(\mathbf{x}_i \right) \right) \\|_{\ell_p},
$$

where $$p = 1$$ or $$2$$, respectively, or a weighted combination of these. Regularized autoencoders have also been considered, wherein the encoder and decoder networks are regularized to either be smooth transformations [12] or learn sparse representations [15]. Although autoencoders are generative models in the sense that the decoders learn to output realistic images given a latent representation, a major limitation is their inability to generate new/unseen images. This can be attributed to the lack of a closed-form or parametric structure to the latent-space distribution, which results in inferior reconstructions of latent-space interpolations. The variational and Wasserstein autoencoder schemes alleviate this issue, by enforcing constraints on the distribution of the latent representations.

### Variational Autoencoders

While AEs consider the encoder output as a *deterministic* representation of the input image, in variational autoencoders (VAEs) [8], the latent representations are assumed to be random, with the objective of modelling the joint distribution $$p_{\text{model}}(\mathbf{x},\mathbf{z})$$, from which the data distribution can be computed as $$p_d(\mathbf{x}) = \int_{\mathcal{Z}} p_{\text{model}}(\mathbf{x},\mathbf{z})\,d\mathbf{z}$$, where $$\mathcal{Z}$$ denotes the support of the latent-space distribution. Invoking Bayes' rule, we have $$p_{\text{model}}(\mathbf{x},\mathbf{z}) = \frac{p_{\text{model}}(\mathbf{x}\|\mathbf{z}) p_z(\mathbf{z})}{p_{\text{model}}(\mathbf{z}\|\mathbf{x})}$$. The decoder can be interpreted as approximating the posterior, *i.e.*, $$\text{Dec}_{\theta}(\mathbf{z}) \sim p_{\text{model}}(\mathbf{x}\|\mathbf{z})$$. As the underlying data distribution is inaccessible, the encoder models an approximation of the likelihood, $$\text{Enc}_{\phi}(\mathbf{x}) \sim q(\mathbf{z}\|\mathbf{x}) \approx p_{\text{model}}(\mathbf{z}\|\mathbf{x})$$. In VAEs, one optimizes the so called evidence lower bound (ELBO), given by

$$
\mathcal{L}_{\text{ELBO}}^{\text{VAE}} = \underbrace{\mathbb{E}_{\mathbf{z}\sim \text{Enc}_{\phi}(\mathbf{x})} \left[ \ln\left(p_{\text{model}}(\mathbf{x}\|\mathbf{z})\right)\right]}_{\text{reconstruction error}} - \underbrace{D_{\text{KL}}\left( q(\mathbf{z}\|\mathbf{x}) \,\\|\, p_z(\mathbf{z})\right)}_{\text{prior matching error}},
$$

where $$D_{\text{KL}}$$ denotes the Kullback-Leibler (KL) divergence between two distributions. The reconstruction error, as in the case of AEs, can be approximated in terms of the $$\ell_2$$ or $$\ell_1$$ loss. The KL divergence can be further simplified by assuming a Gaussian model $$p_z(\mathbf{z})$$, with the latent sample defined by means of a *change-of-variable* formula as $$\mathbf{z}_i = \mu_{\phi}(\mathbf{x}_i)+ \sigma_{\phi}(\mathbf{x}_i)\odot\boldsymbol{\epsilon}$$, where $$(\mu_{\phi},\sigma_{\phi})= \text{Enc}_{\phi}(\mathbf{x}_i)$$ and $$\boldsymbol{\epsilon}$$ is drawn from the standard normal distribution, *i.e.*, $$\boldsymbol{\epsilon}\sim\mathcal{N}(\mathbf{0}_d,\mathbb{I}_d)$$, where in turn, $$\mathbf{0}_d$$ denotes the $$d$$-dimensional null vector, $$\mathbb{I}_d$$ is the $$d$$-dimensional identity matrix, and $$\odot$$ stands for pointwise multiplication. Although VAEs are superior to AEs in terms of enforcing structure on the latent space, generating novel unseen samples remains challenging. Further, the use of the $$\ell_2$$ cost results in over-smoothing of the reconstructed images, owing to the implicit Gaussian prior that this choice of loss introduces [16]. Generative adversarial networks (GANs) [10] were proposed as an alternative to VAEs, to alleviate its limitations.

### Navigation

- **Next:** [Chapter 1.2: Generative Adversarial Networks](/thesis-chapters-p0/2023/05/10/thesis-chapter-1p2-introGANs.html)
- **Back to:** [Thesis Project](/projects/1_thesis)

---

## References

1. **Kang, M., Zhu, J.-Y., Zhang, R., Park, J., Shechtman, E., Paris, S., & Park, T. (2023).** Scaling up GANs for text-to-image synthesis. In *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR)*.

2. **Guo, J., Lu, S., Cai, H., Zhang, W., Yu, Y., & Wang, J. (2018).** Long text generation via adversarial training with leaked information. *Proceedings of the AAAI Conference on Artificial Intelligence*, 32.

3. **Engel, J., Agrawal, K. K., Chen, S., Gulrajani, I., Donahue, C., & Roberts, A. (2019).** GANSynth: Adversarial neural audio synthesis. In *International Conference on Learning Representations (ICLR)*.

4. **Yu, J., Xu, Y., Koh, J. Y., Luong, T., Baid, G., Wang, Z., Vasudevan, V., Ku, A., Yang, Y., Ayan, B. K., Hutchinson, B., Han, W., Parekh, A., Li, X., Zhang, H., Baldridge, J., & Wu, Y. (2022).** Scaling autoregressive models for content-rich text-to-image generation. *arXiv preprint arXiv:2206.10789*.

5. **Kalos, M. H., & Whitlock, P. A. (1986).** *Monte Carlo Methods.* Wiley Publications.

6. **Carlsson, G., Ishkhanov, T., de Silva, V., & Zomorodian, A. (2008).** On the local behavior of spaces of natural images. *International Journal of Computer Vision*, 76(1), 1-12.

7. **Hinton, G. E., & Zemel, R. S. (1994).** Autoencoders, minimum description length and Helmholtz free energy. In *Advances in Neural Information Processing Systems 6 (NIPS 1993)* (pp. 3-10).

8. **Kingma, D. P., & Welling, M. (2013).** Auto-encoding variational Bayes. *arXiv preprint arXiv:1312.6114*.

9. **Tolstikhin, I., Bousquet, O., Gelly, S., & Schoelkopf, B. (2018).** Wasserstein auto-encoders. In *International Conference on Learning Representations (ICLR)*.

10. **Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., Courville, A., & Bengio, Y. (2014).** Generative adversarial nets. In *Advances in Neural Information Processing Systems 27 (NIPS 2014)* (pp. 2672-2680).

11. **Rezende, D., & Mohamed, S. (2015).** Variational inference with normalizing flows. In *International Conference on Machine Learning (ICML)* (pp. 1530-1538).

12. **Liang, K., Chang, H., Cui, Z., Shan, S., & Chen, X. (2015).** Representation learning with smooth autoencoder. In *12th Asian Conference on Computer Vision* (pp. 72-86).

13. **Ho, J., Jain, A., & Abbeel, P. (2020).** Denoising diffusion probabilistic models. In *Advances in Neural Information Processing Systems 33 (NeurIPS 2020)* (pp. 6840-6851).

14. **Rifai, S., Mesnil, G., Vincent, P., Muller, X., Bengio, Y., Dauphin, Y., & Glorot, X. (2011).** Higher order contractive auto-encoder. In *Joint European Conference on Machine Learning and Knowledge Discovery in Databases* (pp. 645-660). Springer.

15. **Ng, A. (2011).** Sparse autoencoder. *CS294A Lecture Notes*, 72, 1-19.

16. **Figueiredo, M. (2001).** Adaptive sparseness using Jeffreys prior. In *Advances in Neural Information Processing Systems* 2001, 14.




