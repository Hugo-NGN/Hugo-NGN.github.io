---
layout: post
title: Kolmogorov Backward Equation (KBE)
date: April 2026
---

# Kolmogorov Backward Equation 
***Hugo NGUYEN, April 2026***

<p align="center">
    <img src="./assets/figure_7_2D_velocity_field.png" alt="multi-brownian-plot" width="1000"/>
</p>

<u>Disclaimer:</u> <i> This document is only a personnal note and may content mistake. It is solely made to summarize and keep track on notions I work on.
</i>

## Introduction

The theory of stochastic processes provides a rigorous mathematical framework to describe the evolution of systems subject to randomness. Among these processes, continuous-time Markov processes play a central role in probability theory, statistical physics, and mathematical finance.

Before introducing the formalism, it is useful to start with a simple intuition. Consider a particle evolving randomly in space (for instance following a Brownian motion with drift). Suppose that we fix a future time $T$, and we associate to the final position $X_T$ a quantity of interest $\varphi(X_T)$ (this could represent a payoff, a cost, or simply an observable of the system).

Now imagine that at an earlier time $t < T$, we observe the system at position $x$. A natural question is:

$$"\textit{What is the expected value of this future quantity, given that we are currently at state } x \textit{ at time } t?"$$

This leads to the definition of the function

$$u(t,x) = \mathbb{E}[\varphi(X_T) \mid X_t = x].$$

This function $u(t,x)$ is the central object of the Kolmogorov Backward Equation. It can be interpreted as a *prediction function*: starting from $(t,x)$, it tells us the average future outcome at time $T$.

<p align="center">
    <img src="./assets/figure_1_many_futures.png" width="700"/>
</p>

<p align="center"><i>
From a single starting point (t,x), many random futures unfold. The function u(t,x) averages all these possible outcomes.
</i></p>

A key point is that $u$ depends on time *backward*: the terminal condition is known at time $T$ (since $u(T,x) = \varphi(x)$), and we want to understand how this information propagates to earlier times.

The Kolmogorov Backward Equation (KBE) provides a partial differential equation satisfied by $u$. It is called "backward" because it evolves backward in time, from the terminal condition at time $T$ to earlier times.

Historically, this equation was introduced by Andrey Kolmogorov in his foundational work on Markov processes. It is intimately connected to the Kolmogorov Forward Equation, also known as the Fokker–Planck equation, which describes the evolution of probability densities. The two equations are adjoint to each other in a suitable functional analytic sense.

<p align="center">
    <img src="./assets/figure_2_forward_backward.png" width="900"/>
</p>

<p align="center"><i>
Left: probability densities evolve forward in time. Right: value functions evolve backward from the terminal condition.
</i></p>

## Kolmogorov Backward Equation

### Probabilistic framework

Let $(\Omega, \mathcal{F}, \mathbb{P})$ be a probability space equipped with a filtration $(\mathcal{F}_t)_{t \geq 0}$ satisfying the usual conditions. Let $(W_t)_{t \geq 0}$ be a $d$-dimensional standard Brownian motion adapted to this filtration.

We consider a stochastic differential equation (SDE) in $\mathbb{R}^n$ of the form

$$dX_t = b(X_t,t)\,dt + \sigma(X_t,t)\,dW_t,$$

with initial condition $X_0 = x \in \mathbb{R}^n$, where

$$b : \mathbb{R}^n \times [0,T] \to \mathbb{R}^n, \quad \sigma : \mathbb{R}^n \times [0,T] \to \mathbb{R}^{n \times d}$$
are measurable functions satisfying conditions ensuring existence and uniqueness of a strong solution.

Intuitively, this equation describes a system that evolves according to two mechanisms: a deterministic drift $b$, which pushes the system in a preferred direction, and a random fluctuation term driven by the Brownian motion $W_t$. The randomness is continuously injected into the system over time.

<p align="center">
    <img src="./assets/figure_3_drift_diffusion.png" width="700"/>
</p>

<p align="center"><i>
The dynamics combine a deterministic drift (vector field) and random fluctuations (diffusion).
</i></p>

The process $(X_t)_{t \geq 0}$ is a time-inhomogeneous Markov process. The Markov property means that the future evolution depends only on the present state, not on the past history. This is precisely why the function $u(t,x)$ is well-defined: knowing $X_t = x$ is enough to characterize the distribution of $X_T$.

Its infinitesimal generator $\mathcal{L}_t$ acts on smooth functions $f \in C^2(\mathbb{R}^n)$ as

$$\mathcal{L}_t f(x) = \sum_{i=1}^n b_i(x,t)\,\partial_{x_i} f(x) + \frac{1}{2} \sum_{i,j=1}^n (a(x,t))_{ij} \,\partial_{x_i x_j}^2 f(x),$$
where $a(x,t) = \sigma(x,t)\sigma(x,t)^\top$.

The generator $\mathcal{L}_t$ can be interpreted as describing the *instantaneous evolution* of expectations. In a heuristic sense, it tells us how a function of the state changes over an infinitesimal time interval.

### Definition of the value function

Let $\varphi : \mathbb{R}^n \to \mathbb{R}$ be a measurable function such that the expectation below is well-defined. Fix a terminal time $T > 0$ and define

$$u(t,x) = \mathbb{E}[\varphi(X_T) \mid X_t = x], \quad 0 \leq t \leq T.$$

It is useful to pause and interpret this definition again. For each point $(t,x)$, we imagine restarting the process at time $t$ from position $x$, letting it evolve randomly until time $T$, applying $\varphi$ to the final state, and averaging over all possible trajectories. The function $u$ is therefore obtained by averaging infinitely many possible futures.

The function $u$ is deterministic due to the Markov property and can be written equivalently as

$$u(t,x) = \mathbb{E}^{t,x}[\varphi(X_T)],$$

where $\mathbb{E}^{t,x}$ denotes expectation for the process starting from $X_t = x$.

### Statement of the Kolmogorov Backward Equation

Under suitable regularity assumptions (e.g. $u \in C^{1,2}([0,T] \times \mathbb{R}^n)$), the function $u$ satisfies the partial differential equation

$$\partial_t u(t,x) + \mathcal{L}_t u(t,x) = 0, \quad (t,x) \in [0,T) \times \mathbb{R}^n,$$
with terminal condition

$$u(T,x) = \varphi(x).$$

This equation expresses a balance: the change of $u$ in time is exactly compensated by the action of the generator. In other words, although individual trajectories are random, the averaged quantity $u$ evolves in a perfectly deterministic way.

Explicitly, this reads

$$\partial_t u(t,x) + \sum_{i=1}^n b_i(x,t)\,\partial_{x_i} u(t,x) + \frac{1}{2} \sum_{i,j=1}^n a_{ij}(x,t)\,\partial_{x_i x_j}^2 u(t,x) = 0.$$

### Derivation via Itô's formula

To derive the KBE, consider the process $u(s, X_s)$ for $s \in [t,T]$. The key idea is the following: if $u$ is indeed the correct prediction function, then along the trajectory of the process, the quantity $u(s,X_s)$ should behave like a *martingale* (its expected future value should remain constant).

<p align="center">
    <img src="./assets/figure_4_martingale.png" width="700"/>
</p>

<p align="center"><i>
Along trajectories, the value function fluctuates randomly but has no systematic drift: it behaves like a martingale.
</i></p>

Applying Itô's formula yields

$$du(s,X_s) = \left(\partial_s u + \mathcal{L}_s u \right)(s,X_s)\,ds + \nabla_x u(s,X_s)^\top \sigma(X_s,s)\,dW_s.$$

The first term corresponds to the deterministic drift of $u(s,X_s)$, while the second term captures its random fluctuations.

Integrating between $t$ and $T$, we obtain

$$u(T,X_T) - u(t,X_t) = \int_t^T \left(\partial_s u + \mathcal{L}_s u \right)(s,X_s)\,ds + \int_t^T \nabla_x u(s,X_s)^\top \sigma(X_s,s)\,dW_s.$$

Taking conditional expectation with respect to $\mathcal{F}_t$, the stochastic integral vanishes, leading to

$$\mathbb{E}[u(T,X_T) \mid \mathcal{F}_t] - u(t,X_t) = \mathbb{E}\left[\int_t^T \left(\partial_s u + \mathcal{L}_s u \right)(s,X_s)\,ds \middle| \mathcal{F}_t \right].$$

Using the terminal condition $u(T,x) = \varphi(x)$ and the definition of $u$, we deduce that the left-hand side is zero. Hence,

$$\mathbb{E}\left[\int_t^T \left(\partial_s u + \mathcal{L}_s u \right)(s,X_s)\,ds \middle| \mathcal{F}_t \right] = 0.$$

For this identity to hold for all initial conditions and trajectories, the integrand must vanish, which leads to
$$\partial_t u + \mathcal{L}_t u = 0.$$

This argument highlights an important idea: the KBE is precisely the condition ensuring that $u(s,X_s)$ has no systematic drift, i.e., it is a martingale.

### Interpretation

The Kolmogorov Backward Equation describes how expectations of future observables propagate backward in time. It provides a bridge between stochastic processes and partial differential equations.

<p align="center">
    <img src="./assets/figure_5_backward_propagation.png" width="700"/>
</p>

<p align="center"><i>
The terminal condition at time T propagates backward in time into a smoother value function.
</i></p>

From an intuitive perspective, instead of simulating many random trajectories to estimate $\mathbb{E}[\varphi(X_T)]$, one can solve a deterministic PDE backward in time. The randomness is thus "integrated out", and all uncertainty is encoded in the coefficients of the equation.

Moreover, the backward equation focuses on *observables* (functions of the state), while the forward equation focuses on *distributions*. These two viewpoints are dual: one evolves functions backward, the other evolves probabilities forward.

<p align="center">
    <img src="./assets/figure_6_monte_carlo.png" width="700"/>
</p>

<p align="center"><i>
Monte Carlo estimation approximates the same quantity as the backward equation, but through simulation instead of a PDE.
</i></p>

## Conclusion

The Kolmogorov Backward Equation is a cornerstone result linking stochastic dynamics and partial differential equations. Starting from a Markov diffusion process defined through a stochastic differential equation, one obtains a deterministic PDE governing conditional expectations of future quantities.

Beyond its formal derivation, its true strength lies in its interpretation: it transforms a problem about random trajectories into a deterministic evolution equation. This perspective is central in many areas such as stochastic control, filtering, and quantitative finance.

An important perspective for future developments lies in high-dimensional systems. In such contexts, directly solving the backward equation becomes intractable due to the curse of dimensionality.