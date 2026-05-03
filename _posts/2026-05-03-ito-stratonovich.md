---
title: "Itô and Stratonovich: Two Ways to Read Noise"
date: 2026-05-03
categories: [notes]
tags: [stochastic-calculus, ito, stratonovich, sde]
permalink: /notes/ito-stratonovich/
---

# Itô and Stratonovich : Two Ways to Read Noise
***Hugo NGUYEN, April 2026***

<p align="center">
	<img src="/assets/images/notes/ito_strato/many_brownian.png" alt="Ito vs Stratonovich integral approximation" width="1000"/>
</p>

<u>Disclaimer:</u> <i> This document is only personnal notes and may content mistake. It is solely made to summarize and keep track on notions I work on.
</i>

## Introduction

Stochastic differential equations provide a natural framework to model dynamical systems subject to randomness. At the core of this theory lies a fundamental difficulty: how should one define integration with respect to a highly irregular signal such as Brownian motion? Unlike classical paths, Brownian trajectories are almost surely nowhere differentiable, which prevents any direct application of standard Riemann–Stieltjes integration.

This difficulty leads to two distinct, yet mathematically consistent, constructions of stochastic integrals: the Itô integral and the Stratonovich integral. These two interpretations correspond to different limiting procedures and encode different notions of how information is used along the trajectory. While they are equivalent up to a correction term, they lead to distinct calculus rules and interpretations.

The purpose of this note is to present both constructions in a rigorous and pedagogical manner, to clarify their differences and their equivalence, and to illustrate these concepts through numerical simulations. Particular attention is given to their respective domains of application: Itô calculus is predominant in mathematical finance due to its non-anticipative nature, whereas Stratonovich calculus appears naturally in physics and has recently gained renewed interest in machine learning.

Throughout this document, we work on a filtered probability space $(\Omega, \mathcal{F}, (\mathcal{F}*t)*{t \geq 0}, \mathbb{P})$ satisfying the usual conditions.

## Brownian Motion and the Problem of Integration

A standard Brownian motion $(W_t)_{t \geq 0}$ is a stochastic process such that $W_0 = 0$ almost surely, it has independent increments, and for all $0 \leq s < t$, $W_t - W_s \sim \mathcal{N}(0, t-s)$. Its trajectories are almost surely continuous.

A fundamental property is its quadratic variation:

<div markdown="0">
$$
[W]*t = \lim*{|\Delta| \to 0} \sum_i (W_{t_{i+1}} - W_{t_i})^2 = t \quad \text{a.s.}
$$
</div>

This implies that Brownian paths are nowhere differentiable, which makes classical integration inapplicable. In particular, the limit of Riemann sums depends on the choice of evaluation points.

A numerical simulation illustrates the irregularity:

```python
import numpy as np
import matplotlib.pyplot as plt

T, N = 1.0, 1000
dt = T / N
t = np.linspace(0, T, N)
W = np.cumsum(np.sqrt(dt) * np.random.randn(N))

plt.plot(t, W)
plt.title("Brownian Motion Sample Path")
plt.xlabel("t")
plt.ylabel("W_t")
plt.show()
```

<p align="center">
	<img src="/assets/images/notes/ito_strato/brownian_motion.png" alt="Brownian motion sample path" width="1000"/>
</p>
<div align="center"><em>Figure 1: Sample path of Brownian motion.</em></div>

## The Itô Integral

Let $(X_t)_{t \geq 0}$ be an adapted process such that

$$
\mathbb{E}\left[\int_0^T X_t^2 dt\right] < \infty.
$$

The Itô integral is defined as the $L^2(\Omega)$-limit:

$$
\int_0^T X_t , dW_t = \lim_{|\Delta| \to 0} \sum_i X_{t_i} (W_{t_{i+1}} - W_{t_i}).
$$

This construction enforces non-anticipativity since $X_{t_i}$ is measurable with respect to $\mathcal{F}_{t_i}$.

A central property is the Itô isometry:

$$
\mathbb{E}\left[\left(\int_0^T X_t , dW_t\right)^2\right] = \mathbb{E}\left[\int_0^T X_t^2 dt\right].
$$

Numerical approximation:

```python
ito_sum = np.sum(W[:-1] * np.diff(W))
print("Ito approximation:", ito_sum)
```

## The Stratonovich Integral

The Stratonovich integral is defined by symmetric approximations:

$$
\int_0^T X_t \circ dW_t = \lim_{|\Delta| \to 0} \sum_i \frac{X_{t_i} + X_{t_{i+1}}}{2} (W_{t_{i+1}} - W_{t_i}).
$$

Here you can see that the Stratonovich integral use the term $X_{t_{i+1}}$ that wasn't for the Itô.
This definition restores the classical chain rule, making it closer to standard calculus.

Numerical approximation:

```python
strat_sum = np.sum((W[:-1] + W[1:]) / 2 * np.diff(W))
print("Stratonovich approximation:", strat_sum)
```

## Itô vs Stratonovich: Difference and Equivalence

The two integrals differ by a correction term. For a smooth function $f$,

$$
\int_0^T f(W_t) \circ dW_t = \int_0^T f(W_t) dW_t + \frac{1}{2} \int_0^T f'(W_t) dt.
$$

This discrepancy arises from the quadratic variation of Brownian motion.

<p align="center">
	<img src="/assets/images/notes/ito_strato/ito_vs_stratonovich.png" alt="Ito vs Stratonovich integral approximation" width="600"/>
</p>
<div align="center"><em>Figure 2: Numerical approximation of Itô and Stratonovich integrals for the same Brownian path. The Itô integral can be negative due to the stochastic nature of the increments.</em></div>

In practice, this difference is reflected directly in numerical simulations. For instance, when approximating both integrals along the same Brownian trajectory, one may obtain values such as $-0.3$ for the Itô integral and $0.2$ for the Stratonovich integral. The difference $0.5$ corresponds precisely to the discrete approximation of the correction term $\frac{1}{2}\int_0^T f'(W_t)\,dt$. 

## Change of Variables: Itô Lemma

Let $f \in C^2(\mathbb{R})$. Then:

$$
df(W_t) = f'(W_t) dW_t + \frac{1}{2} f''(W_t) dt.
$$

In contrast, in the Stratonovich sense:

$$
df(W_t) = f'(W_t) \circ dW_t.
$$

Example with $f(x) = x^2$:

$$
d(W_t^2) = 2W_t dW_t + dt \quad (\text{Itô}),
$$

$$
d(W_t^2) = 2W_t \circ dW_t \quad (\text{Stratonovich}).
$$

## Applications in Finance

In mathematical finance, stochastic integrals are interpreted in the Itô sense. The non-anticipative property ensures that trading strategies depend only on present information. This is consistent with the absence of arbitrage and the construction of risk-neutral measures.

The Black–Scholes model is a canonical example where Itô calculus is essential. In its general form, the price $S_t$ of a risky asset is modeled by the stochastic differential equation

$$
dS_t = \mu S_t \, dt + \sigma S_t \, dW_t,
$$

where $\mu$ is the drift (expected return), $\sigma$ the volatility, and $W_t$ a standard Brownian motion. Under a risk-neutral measure, the drift is replaced by the risk-free rate, and the Itô framework enables the derivation of the associated pricing partial differential equation via Itô’s lemma.

## Stratonovich Calculus in Physics and Machine Learning

Stratonovich integrals arise naturally in the limit of systems with smooth noise approximations. They preserve geometric structures and satisfy the classical chain rule, making them particularly suitable for modeling physical systems.

In machine learning, they appear in continuous-time models such as Neural Stochastic Differential Equations, where invariance and differentiability properties are desirable. A typical Stratonovich formulation of such a model is

$$
dX_t = f_\theta(X_t, t)\, dt + g_\theta(X_t, t) \circ dW_t,
$$

where $f_\theta$ and $g_\theta$ are neural networks parameterized by $\theta$. 

The Stratonovich interpretation is often preferred in this context because it is compatible with standard calculus rules, facilitating backpropagation through continuous dynamics and preserving invariances under smooth transformations of the state space.

Here is a refined version of your section that makes the Hessian / Itô’s lemma connection explicit while staying clean and readable.

## Conversion Between Itô and Stratonovich SDEs

Although Itô and Stratonovich integrals are defined differently, they describe equivalent stochastic processes up to a correction in the drift term. In practice, one can convert from one formulation to the other depending on which is more convenient: Itô calculus is typically preferred for probabilistic analysis, while Stratonovich calculus is often more natural for modeling and geometric reasoning.

The origin of this correction can be understood through Itô’s lemma. For a smooth function $f : \mathbb{R}^d \to \mathbb{R}$, it states that

$$
df(X_t)= \nabla f(X_t)^\top dX_t + \frac{1}{2} \operatorname{Tr}\!\big(\sigma(X_t)\sigma(X_t)^\top \nabla^2 f(X_t)\big)\,dt,
$$

where $\nabla^2 f$ denotes the Hessian. This second-order term has no analogue in classical calculus and is precisely what distinguishes the Itô and Stratonovich interpretations.

#### One-dimensional case

Consider:

$$
dX_t = a(X_t) dt + b(X_t) dW_t.
$$

The equivalent Stratonovich form is:

$$
dX_t = \left(a(X_t) - \frac{1}{2} b(X_t)b’(X_t)\right) dt + b(X_t) \circ dW_t.
$$

This correction term arises from the second-order contribution in Itô’s lemma and can be seen as the one-dimensional manifestation of a Hessian effect.

Here is a clean version with bold vector/matrix notation and slightly tightened rigor/notation.

#### Multidimensional formulation

More generally, consider a $d$-dimensional process $\mathbf{X}_t \in \mathbb{R}^d$ solving

$$
d\mathbf{X}_t = \mathbf{A}(t, \mathbf{X}_t)\,dt + \boldsymbol{\Sigma}(t, \mathbf{X}_t)\,d\mathbf{W}_t,
$$

where:

* $\mathbf{A} : \mathbb{R}^d \to \mathbb{R}^d$ is the drift,
* $\boldsymbol{\Sigma} : \mathbb{R}^d \to \mathbb{R}^{d \times m}$ is the diffusion matrix,
* $\mathbf{W}_t \in \mathbb{R}^m$ is an $m$-dimensional Brownian motion.

The equivalent Stratonovich SDE is

$$
d\mathbf{X}_t= \Big(\mathbf{A}(t, \mathbf{X}_t) - \tfrac{1}{2} \sum_{j=1}^m (\boldsymbol{\Sigma}_j \cdot \nabla)\boldsymbol{\Sigma}_j (t,\mathbf{X}_t)\Big)\, dt + \boldsymbol{\Sigma}(t, \mathbf{X}_t)\circ d\mathbf{W}_t,
$$

where $\boldsymbol{\Sigma}_j$ denotes the j-th column of $\boldsymbol{\Sigma}$.

In coordinates, the correction can be written more explicitly as

$$
\big[(\boldsymbol{\Sigma}_j \cdot \nabla)\boldsymbol{\Sigma}_j\big]_i
= \sum_{k=1}^d \boldsymbol{\Sigma}_{k j}(\mathbf{x}) \, \partial_{x_k} \boldsymbol{\Sigma}_{i j}(\mathbf{x}),
$$

which highlights that it is a directional derivative of the vector field $\boldsymbol{\sigma}_j$ along itself.

This formulation makes the link with Itô’s lemma explicit: the drift correction aggregates the same second-order effects that appear through the Hessian term in the multidimensional Itô formula, but expressed intrinsically in terms of derivatives of the diffusion field.

This makes explicit that the Itô–Stratonovich correction is fundamentally a second-order phenomenon:

* Itô calculus incorporates quadratic variation, leading to Hessian terms,
* Stratonovich calculus preserves the classical chain rule,
* the conversion compensates exactly for this discrepancy.

## Numerical Schemes

The Euler–Maruyama scheme for Itô SDEs:

$$
X_{t+\Delta t} = X_t + a(X_t)\Delta t + b(X_t)\Delta W_t,
$$

is the simplest and most widely used discretization method. It is explicit and easy to implement, but only achieves strong order 1/2 convergence.

For Stratonovich equations, midpoint-based schemes such as Heun’s method are more appropriate, as they better capture the underlying interpretation of the stochastic integral. A typical scheme is

$$
X_{n+1} = X_n + a(X_n)\Delta t + b\!\left(\frac{X_n + \tilde{X}_{n+1}}{2}\right)\Delta W_n,
$$

where $\tilde{X}_{n+1} = X_n + a(X_n)\Delta t + b(X_n)\Delta W_n$ is a predictor step.

This scheme approximates the midpoint evaluation required by the Stratonovich integral. However, it introduces additional computational cost and implicitness through the midpoint term, and its accuracy can degrade when the diffusion coefficient $b(x)$ is highly nonlinear or when time steps are not sufficiently small.

<p align="center">
	<img src="/assets/images/notes/ito_strato/euler_maruyama.png" alt="Euler-Maruyama scheme for Ito SDE" width="1000"/>
</p>
<div align="center"><em>Figure 3: Euler–Maruyama scheme for an Itô SDE (viridis colormap, transparent background).</em></div>

<p align="center">
	<img src="/assets/images/notes/ito_strato/heun_stratonovich.png" alt="Heun scheme for Stratonovich SDE" width="1000"/>
</p>
<div align="center"><em>Figure 4: Heun’s method (midpoint) for a Stratonovich SDE (viridis colormap, transparent background).</em></div>

<p align="center">
	<img src="/assets/images/notes/ito_strato/ito_vs_stratonovich_path.png" alt="Ito vs Stratonovich SDE solution paths" width="1000"/>
</p>
<div align="center"><em>Figure 5: Comparison of solution paths for the same Brownian motion: Euler–Maruyama (Itô) vs Heun (Stratonovich). The difference illustrates the effect of the interpretation of the stochastic integral.</em></div>

## Conclusion

The Itô and Stratonovich integrals provide two consistent frameworks for stochastic integration. The Itô formulation is adapted to probabilistic modeling and finance, while the Stratonovich formulation aligns more closely with classical calculus, geometric reasoning, and differential structure.

Beyond this equivalence, the Stratonovich formulation becomes particularly advantageous in high-dimensional settings, where geometric structure and coordinate invariance play a central role. In such regimes, it provides a natural foundation for constructing latent stochastic representations, where a complex high-dimensional system can be projected onto a lower-dimensional latent space governed by an effective SDE. This perspective is especially relevant in modern machine learning and data-driven modeling, where one seeks compact dynamical representations that preserve the essential stochastic structure of the original system.
