---
layout: post
title: "Thesis Chapter 1.2: Introduction to Generative Adversarial Networks"
date: 2023-05-10 01:00:00
description: "An introduction to GANs and Wasserstein Autoencoders, covering the foundational concepts and variants"
tags: thesis GANs machine-learning generative-models
categories: thesis-chapters-p0
related_posts: true
featured: true
toc:
  beginning: true
---

> *What I cannot create, I do not understand.*  
> — Richard P. Feynman

## Generative Adversarial Networks (GANs)

The original GAN formulation presented by Goodfellow et al. (2014) [1], (hereafter referred to as the Standard GAN, or SGAN) is a *min-max* game between two players --- a generator ($$G$$), and a discriminator ($$D$$), both of which are typically implemented as neural networks. The role of the generator, akin to the decoder in AEs and VAEs, is to create fake samples that mimic the ones coming from the training data distribution. However, the generated samples are not trained with a one-to-one correspondence with the real/target data samples. Instead, the generator is trained to create samples that appear realistic to a *critic*, which is the discriminator network $$D$$ tasked with telling apart the real samples from the fake ones. The optimal $$G$$ is the one that *outsmarts* $$D$$ into confusing the fake samples for real.

We briefly introduce the GAN optimization framework here. An in-depth discussion of various GAN losses and their regularization schemes is presented in Section 1.3, while a comprehensive study of GAN algorithms and training practices for image generation, video synthesis, and neural rendering is provided in recent surveys [2]. The SGAN optimization comprises the generator and the discriminator with respective loss functions. The generator $$G$$ accepts high-dimensional noise $$\mathbf{z} \sim p_z$$ as input and generates fake samples $$\mathbf{x} = G(\mathbf{z}) \sim p_g$$. Unlike in VAEs, the latent space in GANs is not learnt, but assumed to be a standard parametric distribution, typically Gaussian or uniformly distributed in $$\mathbb{R}^{100}$$ or $$\mathbb{R}^{128}$$. The discriminator $$D$$ accepts an input $$\mathbf{x}$$, which could come from either the data distribution $$p_d$$ as a sample from the dataset $$\mathcal{D}$$, or the generator distribution $$p_g$$ as the output of the generator. The discriminator outputs a value $$D(\mathbf{x})$$, which serves as a measure of *how realistic* the input sample $$\mathbf{x}$$ is. Effectively, the generator must learn a mapping from the noise distribution to the data distribution, whereas the discriminator must learn the optimal two-class classifier.

The SGAN discriminator $$D_{\phi}$$ and generator $$G_{\theta}$$ are implemented as neural networks and their parameters $$(\phi,\theta)$$ are optimized as follows:

$$
\min_{\theta} \left\{ \max_{\phi} \Big\{ \mathbb{E}_{\mathbf{x}\sim p_d} \left[\ln\left(D_{\phi}(\mathbf{x})\right)\right] + \mathbb{E}_{\mathbf{z}\sim p_z} \left[ \ln\left(1 - D_{\phi}(G_{\theta}(\mathbf{z}))\right) \right] \Big\} \right\}.
$$

Ideally, the min-max problem can be solved simultaneously to attain the *Nash equilibrium* of the two-player game. In practice, however, alternating gradient ascent-descent algorithms are used to train the networks until a suitable convergence criterion is met.

Over the past decade, numerous variants of GANs have been proposed with several successful applications. From an analytical standpoint, the discriminator in these variants can be viewed as approximating a *measure of dissimilarity* between $$p_d$$ and $$p_g$$. On the other hand, the generator can be viewed as minimizing the measure that the discriminator approximates. Almost all known GAN flavors consider either a divergence or an integral probability metric. We review important GANs under each category.

### Divergence-minimizing GANs

GANs were originally designed to minimize the divergence between the *target* data distribution $$p_d$$ and the generator distribution $$p_g$$. For instance, the SGAN formulation corresponds to optimizing the Jensen-Shannon divergence, whereas the least-squares GAN (LSGAN) [3] has been shown to optimize the Pearson-$$\chi^2$$ divergence under certain assumptions. The $$f$$-GAN formulation [4] is a generalization to any choice of $$f$$-divergence, while other works [5] proposed extensions to various Bregman divergences. In the original SGAN, the discriminator output is constrained to the interval $$[0,1]$$, representing the probability of the input sample being real or fake, whereas LSGAN requires the discriminator output to match a set of chosen class-labels. In $$f$$-GANs, the discriminator output is real-valued, but an appropriately chosen activation function maps it to a desired interval.

### Integral Probability Metric (IPM) GANs

The divergence metric approaches fail if $$p_d$$ and $$p_g$$ are of disjoint support [6], which shifted focus to *integral probability metrics* (IPMs). In these GAN flavors, the *discriminator* is replaced with a *real-valued critic* $$C$$ that differentiates between the generator and data distributions in terms of an integral probability metric (IPM) defined over the class of critics to choose from. The choice of the class of critics gives rise to variants such as the Wasserstein GAN (WGAN) [7] with a Lipschitz-1 critic, the Fisher GAN [8] in which the second-order moments of the critic are bounded or Sobolev GANs [9], which favor critics with a finite energy in the gradient. The critic is a neural network similar to the discriminator, where the constraints are enforced appropriately, either by means of an adjustment of the network weights [7,10,11], or through a suitable penalty incorporated into the loss function [12,13,14]. Throughout this thesis, we use the term *discriminator* $$D(\mathbf{x})$$ to refer to either a divergence-based discriminator or the IPM-based critic with the context and GAN loss function resolving any ambiguity.

## Wasserstein Autoencoders

Wasserstein autoencoders (WAEs) [15] and adversarial autoencoders (AAE) [16] lie at the intersection of AEs and GANs. In these models, a vanilla autoencoder's [17,18] latent space representation is required to conform to a given *prior* distribution, usually a Gaussian or a mixture of Gaussians, through an auxiliary network that minimizes the distance between the two distributions. While VAEs achieve this by means of a loss defined via an evidence lower bound, in AAEs and WAEs, the GAN formulation is leveraged.

The *encoder-decoder* pair is trained by minimizing an appropriate distance measured between the input data distribution and that of the reconstructed samples. In the WAE-GAN setting, the encoder of the WAE also plays the role of the GAN generator, which is trained using a discriminator network to force the latent space distribution to match the prior using a suitable GAN loss. Considering the Euclidean distance metric between a data sample $$\mathbf{x}$$ and the corresponding reconstruction $$\tilde{\mathbf{x}}$$, and the SGAN loss, we obtain the AAE formulation. The vanilla WAE-GAN formulation employs the Euclidean loss for the reconstruction in combination with the KL divergence for the GAN loss. Sliced WAE [19] extended the framework to accommodate the sliced Wasserstein loss [20]. As an alternative to the adversarial formulation, maximum mean discrepancy (MMD) based variations of the WAE have also gained popularity, the most recent of which, the Cramér-Wold autoencoder (CWAE) [21] presents a characteristic kernel that allows for closed-form computation of the distance between the latent distribution and a standard Gaussian. Optimal transport based approaches either approximate the semi-discrete latent-space transportation map with the continuous Bernier potential by drawing links between the latent-space matching in WAEs and the Monge-Ampère partial differential equation [22,23] or determine the Kantorovich potential in a two-step manner to learn a mapping from the source to the target using a WGAN-GP discriminator [24]. Adding regularizers to the discriminator cost in AAEs has been shown to improve the interpolation capabilities of the AE [25].

## A Deep Dive into GANs

Divergence-minimizing GANs consider a discriminator loss that approximates a divergence between $$p_d$$ and $$p_g$$. Table 1 presents the discriminator and generator loss functions of various divergence-minimizing GANs. The standard GAN (SGAN) proposed by Goodfellow et al. (2014) [1] considers both saturating and non-saturating losses. The term saturation refers to the generator gradients vanishing during training. The vanilla SGAN (employing the saturating loss) results in a min-max zero-sum game where the optimal generator minimizes the Jensen-Shannon divergence between $$p_d$$ and $$p_g$$. On the contrary, the SGAN with a non-saturating loss (SGAN-NS) does not readily map to a divergence, but results in a superior performance when training the generator in practice. In the least-squares GAN (LSGAN) [3], one minimizes the squared distance between the discriminator prediction and the class labels $$(a,b,c)$$ for fake, real, and generated samples, respectively. The loss is not symmetric -- the discriminator is trained to assign a class label $$a$$ to the *fakes* and a class label $$b$$ to the *reals*, while the generator loss is designed to promote *confusion* by assigning a label $$c$$ to both the real and fake samples. The optimization objective can be seen as minimizing the Pearson-$$\chi^2$$ divergence when $$b-c=1$$ and $$b-a=2$$.

Nowozin et al. (2016) [4] considered $$f$$-divergences of the form:

$$
\mathfrak{D}_f(p_d\|p_g) = \int_\mathcal{X} p_g(\mathbf{x}) f\left(\frac{p_d(\mathbf{x})}{p_g(\mathbf{x})} \right)\,d\mathbf{x},
$$

where the divergence function $$f: \mathbb{R}_+ \rightarrow \mathbb{R}$$ is convex, lower-semicontinuous and satisfies $$f(1) = 0$$, and $$\mathcal{X}$$ is a suitable domain of integration. They demonstrated that, in a GAN setting, the minimization of $$\mathfrak{D}_f(p_g\|p_d)$$ is equivalent to the sequential minimization of the discriminator and generator losses:

$$
\mathcal{L}_D^{f} = -\mathbb{E}_{\mathbf{x} \sim p_d}[T(\mathbf{x})] + \mathbb{E}_{\mathbf{x} \sim p_g}[f^{c}(T(\mathbf{x}))]\text{, and}
$$

$$
\mathcal{L}_G^{f}= \mathbb{E}_{\mathbf{x} \sim p_{d}}[T^*(\mathbf{x})]-\mathbb{E}_{\mathbf{x} \sim p_g}[f^{c}(T^*(\mathbf{x}))] \text{,}
$$

with respect to $$D$$ and $$p_g$$, respectively, where $$T(\mathbf{x})$$ is the output of the discriminator subjected to activation $$g$$, that is, $$T(\mathbf{x}) = g(D(\mathbf{x}))$$ and $$T^*(\mathbf{x}) = g(D^*(\mathbf{x}))$$, where $$D^*(\mathbf{x})$$ is the optimal discriminator, and $$f^{c}$$ is the Fenchel conjugate of $$f$$. The choice of the divergence $$f$$ and the activation $$g$$ gives rise to GANs that minimize divergences such as Kullback-Leibler (KL), reverse KL, Pearson-$$\chi^2$$, squared Hellinger, Jensen-Shannon, etc.

### Table 1: Divergence-Minimizing GAN Losses

<style>
.table-wrapper table td:first-child { width: 20%; }
</style>

| GAN Variant | Discriminator Loss: $$\mathcal{L}_D$$ | Generator Loss: $$\mathcal{L}_G$$ |
|-------------|---------------------------------------|-----------------------------------|
| **SGAN** | $$\mathcal{L}_D^{\mathrm{S}} = -\mathbb{E}_{\mathbf{x} \sim p_d}[\ln D(\mathbf{x})]-\mathbb{E}_{\mathbf{x} \sim p_g}[\ln (1-D(\mathbf{x}))]$$ | $$\mathcal{L}_G^{\mathrm{S}} = \mathbb{E}_{\mathbf{x} \sim p_d}[\ln D^*(\mathbf{x})]+\mathbb{E}_{\mathbf{x} \sim p_g}[\ln (1-D^*(\mathbf{x}))]$$ |
| **SGAN-NS** | $$\mathcal{L}_D^{\mathrm{NS}} = -\mathbb{E}_{\mathbf{x} \sim p_d}[\ln D(\mathbf{x})]-\mathbb{E}_{\mathbf{x} \sim p_g}[\ln (1-D(\mathbf{x}))]$$ | $$\mathcal{L}_G^{\mathrm{NS}} = -\mathbb{E}_{\mathbf{x} \sim p_d}[\ln (1-D^*(\mathbf{x}))]-\mathbb{E}_{\mathbf{x} \sim p_g}[\ln D^*(\mathbf{x})]$$ |
| **LSGAN** | $$\mathcal{L}_D^{\mathrm{LS}} =\mathbb{E}_{\mathbf{x} \sim p_d}[(D(\mathbf{x})-b)^2]+\mathbb{E}_{\mathbf{x} \sim p_g}[(D(\mathbf{x})-a)^2]$$ | $$\mathcal{L}_G^{\mathrm{LS}} =\mathbb{E}_{\mathbf{x} \sim p_d}[(D^*(\mathbf{x})-c)^2]+\mathbb{E}_{\mathbf{x} \sim p_g}[(D^*(\mathbf{x})-c)^2]$$ |
| **$$f$$-GAN** | $$\mathcal{L}_D^{f} =-\mathbb{E}_{\mathbf{x} \sim p_d}[g(D(\mathbf{x}))] + \mathbb{E}_{\mathbf{x} \sim p_g}[f^{c}(g(D(\mathbf{x})))]$$ | $$\mathcal{L}_G^{f} =\mathbb{E}_{\mathbf{x} \sim p_d}[g(D^*(\mathbf{x}))]-\mathbb{E}_{\mathbf{x} \sim p_g}[f^{c}(g(D^*(\mathbf{x})))]$$ |

**Table 1:** A summary of various divergence minimizing GAN losses. The SGAN and $$f$$-GAN losses are symmetric and lead to a min-max optimization problem, whereas the LSGAN and SGAN-NS are not symmetric. The LSGAN loss is parametrized by class labels $$(a,b,c)$$, wherein the discriminator learns a classifier with the class label $$b$$ for the *reals* and a class label $$a$$ for the *fakes*. The LSGAN generator is trained to *confuse* the discriminator by enforcing the same class label $$c$$ to both the reals and the fakes. Various choices of the Csiszár divergence function $$f$$ (and consequently, its conjugate $$f^c$$), and *activation* function $$g$$ give rise to the family of $$f$$-GANs.

### IPM-Based GANs and Regularization

The divergence metric approaches fail if $$p_d$$ and $$p_g$$ are of disjoint support [6], which shifted focus to *integral probability metrics* (IPMs), where a *critic* function is chosen to approximate a chosen IPM between the distributions [7,8]. Choosing the IPM is equivalent to constraining the class of functions from which the critic/discriminator is drawn. The most popular variant, inspired by optimal transport, is the Wasserstein GAN (WGAN) [7], in which the objective is to minimize the Wasserstein-1 or *earth mover's* distance between $$p_d$$ and $$p_g$$, and the critic is constrained to be Lipschitz-1. Gulrajani et al. (2017) [12] enforced a first-order gradient penalty on the discriminator network to approximate the Lipschitz constraint. Here, the loss is defined via the Kantorovich–Rubinstein duality, subjected to a regularizer $$\Omega_D$$:

$$
\mathcal{L}_D^W = \mathbb{E}_{\mathbf{x} \sim p_d}[D(\mathbf{x})]- \mathbb{E}_{\mathbf{x} \sim p_g}[D(\mathbf{x})] + \lambda_d\Omega_D,
$$

where $$\lambda_d$$ is the Lagrange multiplier associated with the regularizer. Table 2 summarizes the IPM-GAN losses, the corresponding constraint space of the discriminator, and the ideal form of the regularizer $$\Omega_D$$. However, these regularizers are usually replaced with an implementable counterpart, typically involving first-order gradient-based terms [12,14,31,32]. On the other hand, several works [13,27,28,33] showed the empirical success of the first-order gradient penalty in divergence-minimizing GANs. Cramér GANs [34] and Sobolev GANs [9,29] bound the energy in the gradients of $$D$$, enforcing Sobolev-space constraints.

### Table 2: IPM-GAN Losses

<style>
.table-wrapper table td:first-child { width: 20%; }
</style>

| GAN Variant | Unregularized Discriminator Loss | Closed-form Metric: $$\mathfrak{D}$$ | Discriminator Constraint Space |
|-------------|----------------------------------|--------------------------------------|--------------------------------|
| **$$f$$-GANs** | $$-\mathbb{E}_{\mathbf{x} \sim p_d}[T(\mathbf{x})] +\mathbb{E}_{\mathbf{x} \sim p_g}[f^{c}(T(\mathbf{x}))]$$ | $$\mathbb{E}_{\mathbf{x}\sim p_g} \left[ f\left( \frac{p_d(\mathbf{x})}{p_g(\mathbf{x})}\right) \right]$$ | $$\{T = g(D(\cdot)):\,\mathcal{X} \rightarrow \mathbb{R}\,;\,T\in\mathrm{dom}(f)\}$$ |
| **WGAN** | $$\mathbb{E}_{\mathbf{x} \sim p_d}[D(\mathbf{x})]- \mathbb{E}_{\mathbf{x} \sim p_g}[D(\mathbf{x})]$$ | ---- | $$\{D:\,\mathcal{X} \rightarrow \mathbb{R}\,;\,\|D\|_{\mathrm{Lip}}\leq 1 \}$$ |
| **$$\mu$$-Fisher-GAN** | $$\mathbb{E}_{\mathbf{x} \sim p_d}[D(\mathbf{x})]- \mathbb{E}_{\mathbf{x} \sim p_g}[D(\mathbf{x})]$$ | $$\sqrt{ \mathbb{E}_{\mathbf{x}\sim\mu(\mathbf{x})} \left[\left(\frac{p_d(\mathbf{x}) - p_g(\mathbf{x})}{\mu(\mathbf{x})}\right)^2 \right]}$$ | $$\left\{D:\,\mathcal{X}\rightarrow \mathbb{R}\,;\,\mathbb{E}_{\mathbf{x}\sim\mu} \left[D^2(\mathbf{x})\right]\leq 1\right\}$$ |
| **Cramér-GAN** | $$\mathbb{E}_{\mathbf{x} \sim p_d}[D(\mathbf{x})]- \mathbb{E}_{\mathbf{x} \sim p_g}[D(\mathbf{x})]$$ | $$\sqrt{ \mathbb{E}_{x\sim p_g} \left[\left(\frac{\mathrm{CDF}_{p_d}(x) - \mathrm{CDF}_{p_g}(x)}{p_d(x)}\right)^2 \right]}$$ | $$\left\{D:\,\mathcal{X}\rightarrow \mathbb{R}\,;\,\mathbb{E}_{x\sim p_d} \left[ \left(\frac{\partial D(x)}{\partial x}\right)^2\right]\leq 1\right\}$$ |
| **$$\mu$$-Sobolev-GAN** | $$\mathbb{E}_{\mathbf{x} \sim p_d}[D(\mathbf{x})]- \mathbb{E}_{\mathbf{x} \sim p_g}[D(\mathbf{x})]$$ | $$\sqrt{ \mathbb{E}_{\mathbf{x}\sim\mu(\mathbf{x})} \left[ \sum_{i=1}^n\left(\frac{\mathrm{CDF}_{i,p_d}(\mathbf{x}) - \mathrm{CDF}_{i,p_g}(\mathbf{x})}{n\,\mu(\mathbf{x})}\right)^2 \right]}$$ | $$\left\{D:\,\mathcal{X}\rightarrow \mathbb{R}\,;\,\mathbb{E}_{\mathbf{x}\sim\mu} \left[ \|\nabla_{\mathbf{x}} D(\mathbf{x})\|_2^2\right]\leq 1\right\}$$ |
| **MMD-GANs** | $$\mathbb{E}_{\mathbf{x} \sim p_d}[D(\mathbf{x})]- \mathbb{E}_{\mathbf{x} \sim p_g}[D(\mathbf{x})]$$ | $$\left\| \mathbb{E}_{\mathbf{x}\sim p_g} \left[\phi_{k}(\mathbf{x})\right] - \mathbb{E}_{\mathbf{x}\sim p_d} \left[\phi_{k}(\mathbf{x})\right] \right\|_{\mathscr{H}_{\phi_{k}}}$$ | $$\{D:\,\mathcal{X} \rightarrow \mathbb{R}\,;\,\|D\|_{\mathscr{H}_{\phi_{k}}} \leq 1\}$$ |

**Table 2:** A summary of various IPM-GAN losses considered in the literature, compared against an $$f$$-GAN loss. Various choices of the $$f$$-divergence function give rise to the family of $$f$$-GANs. Other GAN variants consider the standard Wasserstein-1 IPM, with varying choices for the discriminator regularizer (the *closed-form metric*), which in turn induces a constraint space for the discriminator. WGAN enforces a Lipschitz-1 discriminator by means of weight-clipping on the network, while Fisher GANs draw discriminators with finite $$L_2$$ norms. While Cramér GANs enforce a gradient-norm penalty in 1-D with a metric defined over the cumulative density functions (CDF), Sobolev GANs consider a generalization involving the sum of all marginal CDFs (indexed by $$i$$). Maximum mean discrepancy GANs (MMD-GANs) derived a kernel-mean statistic associated with the RKHS $$\mathscr{H}_{\phi_{k}}$$.

### Navigation

- **Previous:** [Chapter 1.1: Introduction to Generative Modeling](/thesis-chapters/2023/05/10/thesis-chapter-1p1-IntroGenMod.html)
- **Next:** [Chapter 1.3: Flow-based and Diffusion Models](/thesis-chapters/2023/05/10/thesis-chapter-1p3-FlowsDiffusion.html)
- **Back to:** [Thesis Project](/projects/1_thesis/)

---

## References

1. **Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., Courville, A., & Bengio, Y. (2014).** Generative adversarial nets. In *Advances in Neural Information Processing Systems 27 (NIPS 2014)* (pp. 2672-2680).

2. **Liu, M.-Y., Huang, X., Yu, J., Wang, T.-C., & Mallya, A. (2021).** Generative adversarial networks for image and video synthesis: Algorithms and applications. *Proceedings of the IEEE*, 109(5), 839-862.

3. **Mao, X., Li, Q., Xie, H., Lau, R. Y., Wang, Z., & Smolley, S. P. (2017).** Least squares generative adversarial networks. In *International Conference on Computer Vision (ICCV)* (pp. 2813-2821).

4. **Nowozin, S., Cseke, B., & Tomioka, R. (2016).** f-GAN: Training generative neural samplers using variational divergence minimization. In *Advances in Neural Information Processing Systems 29 (NIPS 2016)* (pp. 271-279).

5. **Tao, C., Chen, L., Henao, R., Feng, J., & Carin, L. (2018).** Chi-square generative adversarial network. In *International Conference on Machine Learning (ICML)* (pp. 4843-4852).

6. **Arjovsky, M., & Bottou, L. (2017).** Towards principled methods for training generative adversarial networks. In *International Conference on Learning Representations (ICLR)*.

7. **Arjovsky, M., Chintala, S., & Bottou, L. (2017).** Wasserstein generative adversarial networks. In *International Conference on Machine Learning (ICML)* (pp. 214-223).

8. **Mroueh, Y., & Sercu, T. (2017).** Fisher GAN. In *Advances in Neural Information Processing Systems 30 (NIPS 2017)* (pp. 2513-2523).

9. **Mroueh, Y., Sercu, T., & Goel, V. (2018).** Sobolev GAN. In *International Conference on Learning Representations (ICLR)*.

10. **Li, Z., Hu, Z., Luo, S., Chen, S., Gao, K., Chen, C., Yang, J., & Xu, B. (2019).** Spectral normalization for generative adversarial networks. In *International Conference on Learning Representations (ICLR)*.

11. **Wang, D., Gong, Z., & Liu, Q. (2016).** Stein variational gradient descent as gradient flow. In *Advances in Neural Information Processing Systems 29 (NIPS 2016)*.

12. **Gulrajani, I., Ahmed, F., Arjovsky, M., Dumoulin, V., & Courville, A. (2017).** Improved training of Wasserstein GANs. In *Advances in Neural Information Processing Systems 30 (NIPS 2017)* (pp. 5767-5777).

13. **Kodali, N., Abernethy, J., Hays, J., & Kira, Z. (2017).** On convergence and stability of GANs. *arXiv preprint arXiv:1705.07215*.

14. **Mescheder, L., Geiger, A., & Nowozin, S. (2018).** Which training methods for GANs do actually converge? In *International Conference on Machine Learning (ICML)* (pp. 3481-3490).

15. **Tolstikhin, I., Bousquet, O., Gelly, S., & Schoelkopf, B. (2018).** Wasserstein auto-encoders. In *International Conference on Learning Representations (ICLR)*.

16. **Makhzani, A., Shlens, J., Jaitly, N., Goodfellow, I., & Frey, B. (2015).** Adversarial autoencoders. *arXiv preprint arXiv:1511.05644*.

17. **Hinton, G. E., & Zemel, R. S. (1994).** Autoencoders, minimum description length and Helmholtz free energy. In *Advances in Neural Information Processing Systems 6 (NIPS 1993)* (pp. 3-10).

18. **Goodfellow, I., Bengio, Y., & Courville, A. (2016).** *Deep Learning.* MIT Press.

19. **Kolouri, S., Pope, P. E., Martin, C. E., & Rohde, G. K. (2019).** Sliced Wasserstein auto-encoders. In *International Conference on Learning Representations (ICLR)*.

20. **Kolouri, S., Zou, Y., & Rohde, G. K. (2018).** Sliced Wasserstein kernels for probability distributions. In *IEEE/CVF Conference on Computer Vision and Pattern Recognition* (pp. 5258-5267).

21. **Patrini, G., van den Berg, R., Forré, P., Carioni, M., Bhargav, S., Welling, M., Genewein, T., & Roth, F. (2020).** Sinkhorn autoencoders. In *Conference on Uncertainty in Artificial Intelligence (UAI)* (pp. 733-743).

22. **Fan, L., Dai, W., Lee, D.-J., & Liu, W. (2019).** On the effectiveness of optimal transport for generative modeling. *arXiv preprint arXiv:1901.11373*.

23. **Patrini, G., van den Berg, R., Forré, P., Carioni, M., Bhargav, S., Welling, M., Genewein, T., & Roth, F. (2020).** Optimal transport autoencoders. In *International Conference on Learning Representations (ICLR)*.

24. **Rubenstein, P., Schoelkopf, B., & Tolstikhin, I. (2018).** On the latent space of Wasserstein auto-encoders. *arXiv preprint arXiv:1802.03761*.

25. **Ghosh, P., Sajjadi, M. S. M., Vergari, A., Black, M., & Schölkopf, B. (2019).** From variational to deterministic autoencoders. *arXiv preprint arXiv:1903.12436*.

26. **Peyré, G., & Cuturi, M. (2019).** Computational optimal transport. *Foundations and Trends in Machine Learning*, 11(5-6), 355-607.

27. **Kodali, N., Abernethy, J., Hays, J., & Kira, Z. (2017).** On convergence and stability of GANs. *arXiv preprint arXiv:1705.07215*.

28. **Kodali, N., Hays, J., Abernethy, J., & Kira, Z. (2017).** How to train your DRAGAN. *arXiv preprint arXiv:1705.07215*.

29. **Adler, J., & Lunz, S. (2018).** Banach Wasserstein GAN. In *Advances in Neural Information Processing Systems 31 (NeurIPS 2018)* (pp. 6754-6763).

30. **Gulrajani, I., Ahmed, F., Arjovsky, M., Dumoulin, V., & Courville, A. (2017).** Improved training of Wasserstein GANs. In *Advances in Neural Information Processing Systems 30 (NIPS 2017)* (pp. 5767-5777).

31. **Petzka, H., Fischer, A., & Lukovnikov, D. (2018).** On the regularization of Wasserstein GANs. In *International Conference on Learning Representations (ICLR)*.

32. **Patel, A. B., Nguyen, T., & Baraniuk, R. G. (2020).** A probabilistic framework for deep learning. In *Advances in Neural Information Processing Systems 29 (NIPS 2016)*.

33. **Mescheder, L., Nowozin, S., & Geiger, A. (2018).** The numerics of GANs. In *Advances in Neural Information Processing Systems 31 (NeurIPS 2018)* (pp. 1825-1835).

34. **Bellemare, M. G., Danihelka, I., Dabney, W., Mohamed, S., Lakshminarayanan, B., Hoyer, S., & Munos, R. (2017).** The Cramer distance as a solution to biased Wasserstein gradients. *arXiv preprint arXiv:1705.10743*.


