---
layout: post
title: "Thesis Chapter 1.3: Flow-based Models and Diffusion"
date: 2023-05-10 02:00:00
description: "An introduction to normalizing flows and score-based diffusion models"
tags: thesis flows diffusion machine-learning generative-models
categories: thesis-chapters-p0
related_posts: true
featured: true
toc:
  beginning: true
---

> *What I cannot create, I do not understand.*  
> — Richard P. Feynman

## Flow-based Models

Flow-based models such as normalizing flows (NFs) [1] leverage the *change-of-variables* formula to learn a transformation from a parametric prior distribution to the target. Unlike in GANs and AEs, in NF models, the input to model $$\mathbf{z}$$ is assumed to be of the same dimensionality as the output, *i.e.*, $$\mathbf{z}\in\mathbb{R}^{n}$$. Normalizing flows model a forward process as the gradual *corruption* of a target distribution $$p_d$$ to the noise distribution $$p_z$$. The forward process is modeled as a series of *invertible* transformations $$f_i$$, such that the composite function yields the desired *push-forward* distribution:

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

where $$\mathbf{z}_i = f_i(\mathbf{z}_{i-1})$$ with $$\mathbf{z}_0 = \mathbf{z}$$ and $$\mathbf{z}_N = \mathbf{x}$$. An in-depth analysis of normalizing flows is presented by recent comprehensive surveys [2]. The $$f_i$$ network architecture is constrained to facilitate easy computation of the Jacobian. Recent approaches design flows based on autoregressive models [3,4,5,6,7], or architectures motivated by the Sobolev GAN loss [9,10]. Flows have also been explored in the context of improving the quality of the noise input in GANs [8,11,12].

## Score-based Diffusion Models

Diffusion models are a recent class of generative models that implement discretized Langevin flow [13], and can be viewed as a random walk in a high-dimensional space. The random walks correspond to a Markov chain, whose stationary distribution converges to the desired distribution. As in the case of flow-based approaches, a forward process is assumed, wherein the data is *corrupted* with noise, while the generative model is an approximation of the reverse process. Langevin Monte Carlo (LMC) is the canonical algorithm for sampling from a given distribution in this framework, where we have access to the *score function*, $$\nabla_{\mathbf{x}}\ln p_d(\mathbf{x})$$, which is the gradient of the logarithm of the target distribution. LMC is an instance of Markov chain Monte Carlo (MCMC) [16], and is a discretization of the Langevin diffusion stochastic differential equation given by

$$
d\mathbf{x}(t) = -\nabla_{\mathbf{x}}f(\mathbf{x}(t))\,dt + \sqrt{2}\,d\mathbf{B}(t),
$$

where $$\mathbf{B}(t)$$ is the standard Brownian motion. The associated discretized update is given by

$$
\mathbf{x}_{t+1} = \mathbf{x}_{t} - \epsilon \left. \nabla_{\mathbf{x}}f(\mathbf{x}) \right|_{\mathbf{x}=\mathbf{x}_t} + \sqrt{2\epsilon}\,\mathbf{z}_t,
$$

where $$\mathbf{z}_{t}$$ is an instance of the noise distribution drawn at time instant $$t$$ and is independent of the sample $$\mathbf{x}_{t}$$ generated at the corresponding time instant. As in normalizing flows, we have $$\mathbf{z}\in\mathbb{R}^{n}$$. The evolution is initialized with parametric noise, *i.e.*, $$\mathbf{x}_0 \sim p_z$$. The choice of the mapping function $$f$$ gives rise to various diffusion models. For example, the popular family of score-based models [14,15] consider $$f(\mathbf{x}) = \nabla_{\mathbf{x}}\ln p_d(\mathbf{x})$$, or in inverse-heat diffusion models [24], we have a frequency-domain transformation corresponding to Gaussian deblurring. In this thesis, we primarily focus on score-based diffusion, and their relation to GAN optimization.

Existing score-based approaches train a neural network $$s_{\theta}(\mathbf{x})$$ to approximate the score, by means of a score-matching loss [17], originally considered in the context of independent component analysis:

$$
\mathcal{L}(\theta)=\frac{1}{2} \,\mathbb{E}_{{\mathbf{x}}\sim p_d} \left[ \left\Vert s_{\theta}(\mathbf{x}) - \nabla_{\mathbf{x}}\ln p_d(\mathbf{x}) \right\Vert_2^2 \right].
$$

The output of the trained network is used to generate samples through the annealed Langevin dynamics in noise-conditioned score networks (NCSN) [14]. However, a major limitation is that the predicted score is *weak* in regions far away from the target distribution, which is typically the case at the start of the Markov chain, around $$\mathbf{x}_0\sim p_z$$. Various approaches such as noise scaling [14], sliced SM [18], and denoising SM [13,15] have been proposed to improve the strength of the gradients. Works considering improved discretization of the underlying differential equation [15,20,21,22] have been developed to accelerate the sampling process. Recently, denoising diffusion GANs (DDGANs) [23] were introduced, wherein a GAN is trained to model the diffusion process, with the generator and discriminator networks conditioned on the time index.

### Navigation

- **Previous:** [Chapter 1.2: Generative Adversarial Networks](/thesis-chapters-p0/2023/05/10/thesis-chapter-1p2-IntroGANs.html)
- **Next:** [Chapter 1.4: An Introduction to Variational Calculus](/thesis-chapters-p0/2023/05/10/thesis-chapter-1p4-VariationalCalculus.html)
- **Back to:** [Thesis Project](/projects/1_thesis/)

---

## References

1. **Rezende, D., & Mohamed, S. (2015).** Variational inference with normalizing flows. In *International Conference on Machine Learning (ICML)* (pp. 1530-1538).

2. **Papamakarios, G., Nalisnick, E., Rezende, D. J., Mohamed, S., & Lakshminarayanan, B. (2021).** Normalizing flows for probabilistic modeling and inference. *Journal of Machine Learning Research*, 22(57), 1-64.

3. **Dinh, L., Krueger, D., & Bengio, Y. (2015).** NICE: Non-linear independent components estimation. In *International Conference on Learning Representations (ICLR) Workshop*.

4. **Dinh, L., Sohl-Dickstein, J., & Bengio, S. (2017).** Density estimation using Real NVP. In *International Conference on Learning Representations (ICLR)*.

5. **Kingma, D. P., & Dhariwal, P. (2018).** Glow: Generative flow using invertible 1×1 convolutions. In *Advances in Neural Information Processing Systems 31 (NeurIPS 2018)* (pp. 10215-10224).

6. **Kingma, D. P., Salimans, T., Jozefowicz, R., Chen, X., Sutskever, I., & Welling, M. (2016).** Improved variational inference with inverse autoregressive flow. In *Advances in Neural Information Processing Systems 29 (NIPS 2016)* (pp. 4743-4751).

7. **Papamakarios, G., Pavlakou, T., & Murray, I. (2017).** Masked autoregressive flow for density estimation. In *Advances in Neural Information Processing Systems 30 (NIPS 2017)* (pp. 2338-2347).

8. **Chen, X., Kingma, D. P., Salimans, T., Duan, Y., Dhariwal, P., Schulman, J., Sutskever, I., & Abbeel, P. (2018).** Variational lossy autoencoder. In *International Conference on Learning Representations (ICLR)*.

9. **Mroueh, Y., Sercu, T., & Goel, V. (2019).** Sobolev descent. In *International Conference on Artificial Intelligence and Statistics (AISTATS)* (pp. 2976-2984).

10. **Gong, C., Wang, D., & Liu, Q. (2020).** Unbiased Sobolev descent. In *International Conference on Machine Learning (ICML)* (pp. 3558-3567).

11. **Kumar, M., Weissenborn, D., & Kalchbrenner, N. (2021).** Regularizing normalizing flows with Kallenberg-Leibler divergence. In *Conference on Uncertainty in Artificial Intelligence (UAI)*.

12. **Dinh, L., Sohl-Dickstein, J., Pascanu, R., & Larochelle, H. (2021).** A RAD approach to deep mixture models. In *International Conference on Learning Representations (ICLR)*.

13. **Ho, J., Jain, A., & Abbeel, P. (2020).** Denoising diffusion probabilistic models. In *Advances in Neural Information Processing Systems 33 (NeurIPS 2020)* (pp. 6840-6851).

14. **Song, Y., & Ermon, S. (2019).** Generative modeling by estimating gradients of the data distribution. In *Advances in Neural Information Processing Systems 32 (NeurIPS 2019)* (pp. 11895-11907).

15. **Song, Y., Sohl-Dickstein, J., Kingma, D. P., Kumar, A., Ermon, S., & Poole, B. (2021).** Score-based generative modeling through stochastic differential equations. In *International Conference on Learning Representations (ICLR)*.

16. **Sinclair, A. (1992).** Improved bounds for mixing rates of Markov chains and multicommodity flow. *Combinatorics, Probability and Computing*, 1(4), 351-370.

17. **Hyvärinen, A. (2005).** Estimation of non-normalized statistical models by score matching. *Journal of Machine Learning Research*, 6, 695-709.

18. **Song, Y., & Ermon, S. (2020).** Improved techniques for training score-based generative models. In *Advances in Neural Information Processing Systems 33 (NeurIPS 2020)* (pp. 12438-12448).

19. **Song, Y., Sohl-Dickstein, J., Kingma, D. P., Kumar, A., Ermon, S., & Poole, B. (2021).** Score-based generative modeling through stochastic differential equations. In *International Conference on Learning Representations (ICLR)*.

20. **Song, Y., Durkan, C., Murray, I., & Ermon, S. (2020).** Maximum likelihood training of score-based diffusion models. In *Advances in Neural Information Processing Systems 33 (NeurIPS 2020)* (pp. 1415-1428).

21. **Jolicoeur-Martineau, A., Li, K., Piché-Taillefer, R., Kachman, T., & Mitliagkas, I. (2021).** Gotta go fast when generating data with score-based models. *arXiv preprint arXiv:2105.14080*.

22. **Karras, T., Aittala, M., Aila, T., & Laine, S. (2022).** Elucidating the design space of diffusion-based generative models. In *Advances in Neural Information Processing Systems 35 (NeurIPS 2022)* (pp. 26565-26577).

23. **Xiao, Z., Kreis, K., & Vahdat, A. (2022).** Tackling the generative learning trilemma with denoising diffusion GANs. In *International Conference on Learning Representations (ICLR)*.

24. **Bansal, A., Borgnia, E., Chu, H.-M., Li, J., Kazemi, H., Huang, F., Goldblum, M., Geiping, J., & Goldstein, T. (2023).** Cold diffusion: Inverting arbitrary image transforms without noise. In *Advances in Neural Information Processing Systems 36 (NeurIPS 2023)*.
