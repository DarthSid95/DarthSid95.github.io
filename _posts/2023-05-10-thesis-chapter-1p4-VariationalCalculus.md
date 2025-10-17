---
layout: post
title: "Thesis Chapter 1.4: An Introduction to Variational Calculus"
date: 2023-05-10 03:00:00
description: "An introduction to the Calculus of Variations and the Euler-Lagrange framework"
tags: thesis calculus-of-variations euler-lagrange optimization
categories: thesis-chapters
related_posts: true
featured: true
toc:
  beginning: true
---

> *What I cannot create, I do not understand.*  
> — Richard P. Feynman

## Calculus of Variations

The cornerstone of this thesis is the **Euler-Lagrange (EL) framework**, which is at the heart of **Calculus of Variations**. The EL conditions are of fundamental importance in solving several problems in physics and serve as a functional equivalent to the variable optimization performed in univariate or multivariate Calculus.

Consider the 1-D integral cost

$$
\mathcal{L}\left( y(x), y^{\prime}(x)\right)=\int \limits_{a}^b \mathcal{F}\left(x, y(x), y^{\prime}(x)\right)\,dx,
$$

to be optimized with respect to $$y(x)$$, $$x \in [a,b] \subset \mathbb{R}$$, which is assumed to be continuously differentiable or at least continuous with a piecewise-smooth derivative $$y^{\prime} = \frac{dy}{dx}$$, with finite Dirichlet boundary conditions (*i.e.*, the function vanishes on the boundary at $$x=a$$ and $$x=b$$). Let $$y^*(x)$$ denote the optimizer of $$\mathcal{L}$$. The *first variation* of $$\mathcal{L}$$, evaluated at $$y^*$$, is defined as the derivative $$\delta\mathcal{L}(y^*;\eta) = \frac{\partial\mathcal{L}_{y,\epsilon}(\epsilon)}{\partial\epsilon}$$ evaluated at $$\epsilon=0$$, where $$\mathcal{L}_{y,\epsilon}(\epsilon)$$ denotes an $$\epsilon$$-perturbation of the argument $$y$$ about the optimum $$y^*$$, given by

$$
\begin{align*}
  \mathcal{L}_{y,\epsilon}(\epsilon) &= \mathcal{L}\left( y^*(x) + \epsilon\,\eta(x), y^{*\prime}(x) + \epsilon \,\eta^{\prime}(x)\right) \\
&= \int \limits_a^b \mathcal{F}\left(x, y^*(x) + \epsilon\,\eta(x), y^{*\prime}(x) + \epsilon \,\eta^{\prime}(x)\right)\,dx,
\end{align*}
$$

where, in turn, $$\eta(x)$$ is a family of compactly supported, infinitely differentiable functions that are identically zero at the boundary $$x = a$$ and $$x=b$$. The key idea in variational Calculus is the reformulation of a functional optimization over $$y(x)$$ into a variable one, over $$\epsilon$$.

Another key concept, the *fundamental lemma of Calculus of Variations* states that if a function $$f(x)$$ satisfies the condition

$$
\int_{\mathcal{X}} f(x)\,\eta(x)\,dx = 0
$$

for all compactly supported, infinitely differentiable functions $$\eta(x)$$, then $$f$$ must be identically zero almost everywhere in $$[a,b]$$. Setting the first variation to zero and invoking the fundamental lemma of Calculus of Variations gives rise to the Euler-Lagrange condition.

The Euler-Lagrange condition that the optimizer $$y^*(x)$$ must satisfy is given as follows:

$$
\frac{\partial\mathcal{F}}{\partial y} - \frac{\partial}{\partial x} \left(\frac{\partial\mathcal{F}}{\partial y^\prime} \right)\Bigg\vert_{y=y^*(x)}=0.
$$

In the special case where the cost $$\mathcal{L}$$ does not involve the derivative of $$y$$, the EL condition reduces to the simplified form:

$$
\frac{\partial\mathcal{F}}{\partial y}\Bigg|_{y=y^*(x)}=0,
$$

which corresponds to a point-wise optimization with respect to $$y$$, over the interval $$[y(a),y(b)]$$.

## The Multivariate Case

In the multivariate case, that is, $$\mathbf{x} \in \mathbb{R}^n$$, the cost is of the type

$$
\mathcal{L}\left(y\left(\mathbf{x}\right), \left\{y^{\prime}_{i}\right\}_{i=1}^{n}  \right) =\int \limits_{\mathcal{X} \subseteq \mathbb{R}^n} \mathcal{F}\left(\mathbf{x}, y, \left\{y^{\prime}_{i} \right\}_{i=1}^{n}\right)\,d\mathbf{x},
$$

where $$\mathcal{X}$$ is the domain of integration and $$y^{\prime}_{i}$$ denotes the partial derivative of $$y(\mathbf{x})$$ w.r.t. the $$i^{\mathrm{th}}$$ entry of $$\mathbf{x}$$, that is, $$x_i$$, and the boundary is defined as $$\partial\mathcal{X}$$. The corresponding EL condition is

$$
\frac{\partial \mathcal{F}}{\partial y} - \sum_{i=1}^{n} \left[ \frac{\partial}{\partial x_i} \left( \frac{\partial\mathcal{F}}{\partial y^{\prime}_{i}} \right)\right] \Bigg|_{y=y^*(\mathbf{x})}= 0.
$$

The EL condition is a first-order condition and enforcing it yields the optimum. Whether the optimum corresponds to a minimizer or maximizer of the cost must be checked by invoking the second-order condition, more specifically the Legendre-Clebsch necessary condition for a minimizer. In the 1-D case, the condition is given by $$\frac{\partial^2 \mathcal{F}}{\partial y^{\prime 2}} \geq 0.$$

In the multivariate setting, this condition translates to the positive-semi-definiteness (p.s.d.) of the Hessian matrix $$\mathbb{H}$$ of the Hamiltonian $$\mathcal{H}$$, computed with respect to $$\{y^{\prime}_i(\mathbf{x})\}_{i=1}^n$$ and evaluated at $$y(\mathbf{x}) = y^*(\mathbf{x})$$: $$\mathbb{H}_{y,\mathcal{H}}\big|_{y=y^*} \succeq 0$$, where $$\succeq$$ denotes the p.s.d. property. The Hamiltonian is given by

$$
\mathcal{H} = \sum_{i=1}^n \left( y_i^{\prime} \frac{\partial\mathcal{F}}{\partial y_i^{\prime}} \right) - \mathcal{F},
$$

and the entries of the Hessian are given by

$$
[\mathbb{H}_{y,\mathcal{H}}]_{i,j} = \frac{\partial^2 \mathcal{H}}{\partial y_i^{\prime}\partial y_j^{\prime}}.
$$

As in the 1-D case, when the integral cost $$\mathcal{L}$$ (and the integrand $$\mathcal{F}$$) do not depend on the derivatives $$\{y_i^{\prime}\}$$, we have the simplified form of the EL condition:

$$
\frac{\partial\mathcal{F}}{\partial y}\Bigg|_{y=y^*(\mathbf{x})}=0,
$$

which is identical to the point-wise optimization seen in the scalar case. Similarly, when $$\frac{\partial\mathcal{F}}{\partial x} = 0$$, we have the Beltrami identity $$\mathcal{F} - y^{\prime}\frac{\partial\mathcal{F}}{\partial y^{\prime}} = 0$$.

## The Brachistochrone Problem



The **Brachistochrone** or "shortest time" problem is considered to be one of the earliest known formulations of a variational problem, the solution to which supposedly gave rise to the field of Variational Calculus. The problem was posed by Johann Bernoulli in 1696 as follows: *"Given two points on a plane at different heights, what is the shape of the wire down which a bead will slide (without friction) under the influence of gravity so as to pass from the upper point to the lower point in the shortest amount of time?"* Isaac Newton and Jakob Bernoulli showed that the solution is a *cycloid*. The approach adopted by them was eventually formalized into the *Calculus of Variations* [1].

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/thesis/Brachistochrone/Problem.pdf" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/thesis/Brachistochrone/Solution.pdf" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Visualizations of the Brachistochrone problem. Given two points at different heights, we seek the curve that minimizes the travel time of a bead sliding under gravity without friction. The optimal solution to the Brachistochrone problem, the cycloid, results in the shortest travel time compared to other curves such as a straight line or polynomial path connecting the two points.
</div>

### The Solution

Given a curve $$y(x)$$ joining the points $$\mathrm{A} = (0,a)$$ and $$\mathrm{B} = (b,0)$$, we can define the boundary conditions as $$y(0) = a$$ and $$y(b) = 0$$. The speed of a particle on the curve is given by $$v = \frac{ds}{dt} = \sqrt{\left(\frac{dx}{dt}\right)^2 + \left(\frac{dy}{dt}\right)^2}$$, where $$s$$ is the arc length. Then, we have $$ds = \sqrt{1 + (y^{\prime})^2}\,dx$$. From the law of conservation of energy, under the action of gravity, we have $$v = \sqrt{2gy}$$, where $$g$$ is the acceleration due to gravity. Let the curve between $$\mathrm{A}$$ and $$\mathrm{B}$$ be of length $$s_y$$, traversable in time $$t_y$$. The functional optimization problem is given by:

$$
y^*(x) = \arg\min_{y} \left\{ \int_{0}^{t_f} dt = \int_{0}^{s_y} \frac{1}{v}ds = \frac{1}{\sqrt{2g}}\int_{0}^{b} \sqrt{\frac{1 + (y^{\prime})^2}{y}} dx \right\}
$$

Enforcing the Euler-Lagrange condition (and in particular, the Beltrami identity) yields:

$$
\left(1 + (y^{\prime}(x))^2 \right) y(x) \big|_{y(x) = y^*(x)} = k,
$$

for some constant $$k$$. The above differential equation can be solved in polar coordinates, to result in the family of solutions

$$
x = \frac{k}{2} \left( \theta - \sin(\theta)\right) \quad \text{and} \quad y = \frac{k}{2} \left( 1 - \cos(\theta)\right),
$$

which are parametric equations of a cycloid.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/Brachistochrone.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true muted=true %}
    </div>
</div>
<div class="caption">
    Animation of the Brachistochrone problem showing the cycloid path as the optimal solution for minimizing travel time under gravity.
</div>

## The Higher-Order Euler-Lagrange Condition

We now recall the generalization to the EL condition, considering higher-order gradient penalties in $$n$$-D.

Consider a vector $$\mathbf{x} = [x_1, x_2,\,\ldots\,,x_n]^{\mathsf{T}} \in \mathbb{R}^n$$ and a function $$y:\mathbb{R}^n \rightarrow \mathbb{R}$$. The notation $$\nabla_{\mathbf{x}}^m y(\mathbf{x})$$ denotes the vector of $$m^{\mathrm{th}}$$-order partial derivatives of $$y$$ with respect to the entries of $$\mathbf{x}$$. We drop the subscript $$\mathbf{x}$$ for convenience. $$\nabla^0$$ is the identity operator. The elements of $$\nabla^m y$$ are represented using the multi-index $$\boldsymbol{\alpha} = [\alpha_1, \alpha_2,\,\ldots\,,\alpha_n]^{\mathsf{T}}$$, as:

$$
\partial^{\boldsymbol{\alpha}} y = \frac{\partial^{|\boldsymbol{\alpha}|}}{\partial x_1^{\alpha_1}\partial x_2^{\alpha_2}\ldots \partial x_n^{\alpha_n} } y,\quad\text{where}\quad\boldsymbol{\alpha} \in \mathbb{Z}_{*}^n,\quad |\boldsymbol{\alpha}| = \sum_{i=1}^n \alpha_i,
$$

where in turn $$\mathbb{Z}_{*}^n$$ is the set of $$n$$-dimensional vectors with non-negative integer entries. For example, with $$n=4,m=3$$, the index $$\boldsymbol{\alpha} = [2,0,0,1]^{\mathsf{T}}$$ yields the element $$\frac{\partial^3}{\partial x_1^2 \partial x_4}y(\mathbf{x})$$. Within this formulation, the higher-order gradient vector $$\nabla^m y$$ can be operated on similar to the classical first-order gradient vector $$\nabla y$$. For example, the square of the $$\ell_2$$-norm of $$\nabla^m y$$ is given by a multidimensional sum:

$$
\| \nabla^m y(\mathbf{x})\|_2^2 = \sum_{\boldsymbol{\alpha}:~|\boldsymbol{\alpha}| = m} \left( \frac{m!}{\boldsymbol{\alpha}!}\right) \left(\partial^{\boldsymbol{\alpha}} y(\mathbf{x})\right)^2,
$$

where $$\boldsymbol{\alpha}! = \alpha_1!\alpha_2!\,\ldots\,\alpha_n!$$.

### Higher-Order EL Condition

Consider an integral cost $$\mathcal{L}$$ with the integrand $$\mathcal{F}$$ dependent on $$y$$ and all its partial derivatives up to and including order $$\ell$$, given by

$$
\mathcal{L}\left(y(\mathbf{x}),\partial^{\boldsymbol{\alpha}} y;|\boldsymbol{\alpha}|\leq \ell  \right) = \int_{\mathcal{X}} \mathcal{F} \left( y(\mathbf{x}),\partial^{\boldsymbol{\alpha}} y;|\boldsymbol{\alpha}|\leq \ell \right) d\mathbf{x},
$$

defined on a suitable domain $$\mathcal{X}$$ over which $$y$$ and its partial derivatives up to and including order $$\ell$$ are continuously differentiable.

Consider the perturbations $$y(\mathbf{x})= y^*(\mathbf{x}) + \epsilon \eta(\mathbf{x})$$, characterized by $$\eta(\mathbf{x})$$ drawn from the family of compactly supported and infinitely differentiable functions. Then, the second-order Taylor-series approximation of $$g(\epsilon) = \mathcal{L}\left(y(\mathbf{x})\right) = \mathcal{L}\left(y^*(\mathbf{x}) + \epsilon \eta(\mathbf{x})\right)$$ is given by

$$
\begin{align*}
g(\epsilon) &= \mathcal{L}\left(y^*(\mathbf{x}) + \epsilon \eta(\mathbf{x})\right) \\
&= \mathcal{L}\left(y^*(\mathbf{x})\right) + \epsilon\,\delta\mathcal{L}(y^*;\eta) + \frac{1}{2} \epsilon^2\, \delta^2\mathcal{L}(y^*;\eta),
\end{align*}
$$

where $$\delta\mathcal{L}(\cdot)$$ and $$\delta^2\mathcal{L}(\cdot)$$ denote the first variation and second variation of $$\mathcal{L}$$, respectively, and can be evaluated through the scalar optimization problems [2]:

$$
\delta\mathcal{L}(y^*(\mathbf{x})) = g^{\prime}(0) =  \frac{\partial g(\epsilon)}{\partial\epsilon} \bigg|_{\epsilon = 0},
$$

and

$$
\delta^2\mathcal{L}(y^*(\mathbf{x})) = g^{\prime\prime}(0) =  \frac{\partial^2 g(\epsilon)}{\partial\epsilon^2} \bigg|_{\epsilon = 0}.
$$

Evaluating the first variation $$\delta\mathcal{L}(y^*;\eta) = \frac{\partial\mathcal{L}_{y,\epsilon}(\epsilon)}{\partial\epsilon}$$ at $$\epsilon=0$$ yields the Euler-Lagrange condition that the optimizer $$y^*$$ must satisfy:

$$
\left. \frac{\partial \mathcal{F}}{\partial y} + \sum_{j = 1}^{\ell} \left( (-1)^j \sum_{ \boldsymbol{\alpha}:\,|\boldsymbol{\alpha}| = j} \partial^{\boldsymbol{\alpha}} \left( \frac{\partial \mathcal{F}}{\partial ( \partial^{\boldsymbol{\alpha}} y )}\right) \right) \right|_{y= y^*} = 0.
$$

As before, the EL condition merely provides us the extrema, and the second-order conditions must be checked to ascertain if the extrema is a minimizer or maximizer. The second-order Legendre condition for $$y^*(\mathbf{x})$$ to be a minimizer is $$g^{\prime\prime}(0) > 0$$ [2].

This general higher-order form reduces to the standard first-order condition when $$\ell=1$$, and allows us to handle regularized optimization problems involving higher-order derivatives, which will be relevant in the analysis of gradient-penalized GANs and regularized discriminators.

## Connection to Generative Adversarial Networks

The Euler-Lagrange framework provides a powerful mathematical foundation for analyzing GANs from a variational perspective. In the context of GANs:

- The **discriminator optimization** can be viewed as a functional optimization problem where we seek the optimal function $$D^*(\mathbf{x})$$ that maximizes a divergence or IPM between distributions. *Regularized GANs* that incorporate gradient penalties (such as WGAN-GP [3], Sobolev GAN [4]) naturally fit into the higher-order Euler-Lagrange framework, where the regularizers constrain the class of admissible discriminator functions. The simplified EL condition (when derivatives are not involved) can be used to analyze unregularized GAN formulations, while the full EL condition with gradient terms will be essential for understanding regularized GANs. This variational perspective allows us to:
    1. Derive closed-form expressions for optimal discriminators
    2. Analyze the effect of various regularization schemes
    3. Connect GAN optimization to classical problems in signal processing and interpolation theory

- The **generator optimization** seeks the optimal transformation $$G^*(\mathbf{z})$$ that minimizes a loss given the optimal discriminator. What will also be key in this setting, in the time-evolving nature of the generator, as the discriminator changes between iterations. We can further also use the EL setting to understand how GANs link to flow- and score-based diffusion models!!


### Navigation

- **Previous:** [Chapter 1.3: Flow-based and Diffusion Models](/thesis-chapters/2023/05/10/thesis-chapter-1p3-FlowsDiffusion.html)
- **Next:** [Chapter 2: Divergence-Minimizing GANs](/blog/2023/thesis-chapter-2-DivergenceGANs/)
- **Back to:** [Thesis Project](/projects/1_thesis/)

---

## References

1. **Goldstine, H. H. (1980).** *A History of the Calculus of Variations from the 17th through the 19th Century*. Springer-Verlag.

2. **Gelfand, I. M., & Fomin, S. V. (1963).** *Calculus of Variations*. Prentice-Hall.

3. **Gulrajani, I., Ahmed, F., Arjovsky, M., Dumoulin, V., & Courville, A. (2017).** Improved training of Wasserstein GANs. In *Advances in Neural Information Processing Systems 30 (NIPS 2017)* (pp. 5767-5777).

4. **Mroueh, Y., Sercu, T., & Goel, V. (2018).** Sobolev GAN. In *International Conference on Learning Representations (ICLR)*.

5. **Jost, J., & Li-Jost, X. (1998).** *Calculus of Variations*. Cambridge University Press.

6. **Dacorogna, B. (2007).** *Introduction to the Calculus of Variations* (2nd ed.). Imperial College Press.

7. **Evans, L. C. (2010).** *Partial Differential Equations* (2nd ed.). American Mathematical Society.
