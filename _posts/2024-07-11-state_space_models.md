---
layout: post
title: State Space Models
date: 2024-07-11
description: State Space Models
tags: ml, algorithm, SSM
comments: true
---
<style>
    #tableOfContents {
        font-size: 1.6em;
    }
</style>

<details style="background-color: #F5F5F5;">
<summary id="tableOfContents">Table of Contents</summary>
<ul style="font-size:1.4em">
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#continuous-state-space-model">Continuous State Space Model</a></li>
  <li><a href="#derive-hippo-matrices">Derive HiPPO Matrices</a></li>
    <ul>
        <li><a href="#setup">Setup</a></li>
        <li><a href="#approximation">Approximation</a></li>
        <li><a href="#orthogonal-polynomials">Orthogonal Polynomials</a></li>
    </ul>
</ul>
</details>

<br>

# Introduction
This document provides the mathematic derivations of the state space models (SSMs) that help readers understand the formulas used in many SSMs related work. The contents are extracted and summarized from the original papers.

<br>

# Continuous State Space Model

Consider a continuous state space model equation: 

$$
\label{c_ssm}
\tag{1}
\Large
\begin{aligned}
\dot{h}(t)=Ah(t)+Bx(t) \\ 
y(t)=Ch(t)+Dx(t)
\end{aligned}
$$

There are three continuous signals in the system: $$x(t), h(t), y(t)$$,
which are:

1.  $$x(t)\in \mathbb{R}^p$$ is the `input (or control) vector` that
    describes the input continuous signal. 
    Some papers would use $$u$$ as the input vector, but we use $$x$$ for better aligning with Mamba series.

2.  $$h(t)\in \mathbb{R}^n$$ is the `state vector` that describes the system
    states, and $$\dot{h}$$ is the system dynamics *i.e.*
    $$\frac{\partial h(t)}{\partial t}$$.
    Some papers would use $$x$$ as the state vector, but we use $$h$$ for better aligning with Mamba series.

3.  $$y(t) \in \mathbb{R}^q$$ is the `output vector` that describes the
    output continuous signal from the system.

There are four transition matrices in the system: $$A, B, C,$$ and $$D$$,
which are:

1.  $$A, \texttt{Dim}(A)=n\times n$$ is the `state matrix` of the system that
    describes the linear relations of the parameters in the system.

2.  $$B, \texttt{Dim}(B)=n\times p$$ is the `input matrix` that describes how
    input signals interact with the system.

3.  $$C, \texttt{Dim}(C)=q\times n$$ is the `output matrix` that describes the
    system output.

4.  $$D, \texttt{Dim}(D)=q\times p$$ is the `feed-forward matrix` that
    describes how input signals are directly involved in the output.

<br>

# Derive HiPPO Matrices

## Setup
To avoid confusion with the conventional variables in the following derivations and to follow the original derivation in the HiPPO paper, we repalce $$h(t)$$ with $$c(t)$$ and $$x(t)$$ with $$f(t)$$ in the state dynamic equation in $$\eqref{c_ssm}$$.

HiPPO considers a linear time-invariant (LTI) ODEs such that

$$
\Large
\begin{aligned}
\dot{c}(t)=Ac(t)+Bf(t) .
\end{aligned}
$$

1.  $$c(t)$$ is the coefficient vector that describes the combination of
    the basis functions, which corresponds to the state vector $$h(t)$$ in the continuous state space
    model $$\eqref{c_ssm}$$.

2.  $$f(t)$$ is the input signal that corresponds to the input vector $$x(t)$$ in the continuous state
    space model $$\eqref{c_ssm}$$.
    
3.  $$\dot{c}(t)$$ is the system dynamics that represents a compression of the history of $$f$$ that satisfies
    linear dynamics.




<br>
## Approximation

Given a time-varying measure family $$u^{(t)}$$ supported on $$( -\inf, t]$$,  a sequence of basis functions $$\mathcal{G} = \text{span} \{ {g_n}^{(t)} \}_{n \in [N]}$$]
, and a continuous function $$f: \mathbb{R}_{\geq0} \mapsto \mathbb{R}$$, HiPPO defines an operator that maps $$f$$ to the optimal projection coefficients $$c: \mathbb{R}_{\geq0}\mapsto \mathbb{R}^N$$, such that

$$
\Large
\begin{aligned}
    g^{(t)} = \text{argmin}_{g \in \mathcal{G}}\left\lVert f_{x \leq t} - g\right\rVert_{\mu^{(t)}}
\end{aligned}

\quad  \text{and} \quad

\Large
\begin{aligned}
    g^{(t)} = \sum^{N-1}_{n=0} c_n(t) g_n^{(t)}
\end{aligned}
$$


<br>
## Measures

At every $$t$$, the approximation quality is defined with respect to a measure $$\mu^{(t)}$$ supported on $$(-\infty, t]$$. We seek some polynomial $$g^{(t)}$$ of degree at most $$N-1$$ that minimizes the error $$\begin{aligned}
    \left\lVert f_{x \leq t} - g^{(t)}\right\rVert_{L_2(\mu^{(t)})}
\end{aligned}$$.
Suppose the measures $$\mu^{(t)}$$ are sufficiently smooth across their domain as well as in time and have desities
$$
\begin{aligned}
\omega(t, x) = \frac{d\mu^{(t)}}{d\lambda}(x)
\end{aligned}
$$
with respect to the Lebesgue measure $$d\lambda(x)=dx$$
such that $$\omega$$ is $$C^1$$ almost everywhere.
Therefore, we have 
$$
\begin{aligned}
\omega(t, x) = \frac{d\mu^{(t)}(x)}{dx}
\end{aligned}
$$.
Thus, integrating against
$$d\mu^{(t)}(x)$$ can be rewritten as integrating against
$$\omega(t, x)dx$$, such that
$$
\begin{aligned}
\int d\mu^{(t)}(x) = \int \omega(t, x)dx
\end{aligned}
$$.


<br>
## Orthogonal Polynomial Basis

We can use a sequence of orthogonal polynomials (OPs) as our basis to approximate the continuous function $$f$$.
A sequence of OPs $$P_0(x), P_1(x), ...$$ satisfying:

1. $$\text{deg}(P_i)=i$$, and

2. $$\langle P_i, P_j \rangle = \int P_i(x) P_j(x) d\mu(x) = 0$$ for all $$i \neq j$$

A sequence of OPs $$g = \{P_n\}_{n \in \mathbb{N}}$$ of degree $$\text{deg}(g)<N$$ that approximate a function $$f$$ is
then given by

$$\begin{aligned}
    \sum_{i=0}^{N-1} c_i \frac{P_i(x)}{\left\lVert P_i\right\rVert^2_\mu} \quad \text{where } c_i = \langle f, P_i \rangle = \int f(x)P_i(x)d\mu(x)
\end{aligned}$$.

This projects an input sequence signal onto a sequence of OPs, and $$c_i$$ describes the magnitude of
the $$P_i$$ to reconstruct the input signal.

Let $$\{P_n\}_{n \in \mathbb{N}}$$ denote a sequence of OPs with respect
to some base measure $$\mu$$.
Similarly, define
$$ \{ {P_n}^{(t)} \}_{n \in \mathbb{N}}$$ 
to be a sequence of OPs with
respect to the time-varying measure $$\mu^{(t)}$$. 
Let $${p_n}^{(t)}$$ be the normalized version of $${P_n}^{(t)}$$ (*i.e.,* have norm 1), and define
$$\begin{aligned}
p_n(t, x) = {p_n}^{(t)}(x)
\end{aligned}$$.
The $${P_n}^{(t)}$$ are not required to be normalized, while the
$${p_n}^{(t)}$$ are.


<br>
## Tilted Measure and Basis
The goal is simply to store a compressed representation of functions, which
can use any basis, not necessarily OPs. For any scaling function
$$\begin{aligned}
    \chi(t, x) = \chi^{(t)}(x)
\end{aligned}$$
such that $$p_n(x)\chi(x)$$ are orthogonal with respect to
the density $$\omega/\chi^2$$ at every time $$t$$.
Thus, we can choose this alternative basis and measure to perform the projections.

Define $$\nu$$ to be the normalized measure with density proportional to
$$\omega^{t}/(\chi^{(t)})^2$$.
We will calculate the normalized measure and the orthonormal basis for it.

Let $$\zeta(t)$$ to be the normalization
constant: $$\begin{aligned}
    \zeta(t) = \int \frac{\omega}{\chi^2} = \int \frac{\omega^{(t)}(x)}{({\chi^{(t)}(x)})^2}dx
\end{aligned}$$ , so that $$\nu^{(t)}$$ has density
$$\frac{\omega^{(t)}(x)}{\zeta(t)(\chi^{(t)}(x))^2}$$
If $$\chi(t, x) = 1$$ (no tilting), this constant is $$\chi(t) = 1$$.

We define the orthogonal basis for $$\nu^{(t)}$$

$$
\Large
\begin{aligned}
    {g_n}^{(t)} = \lambda_n \zeta(t)^{\frac{1}{2}} {p_n}^{(t)} \chi^{(t)}, \quad n \in \mathbb{N}
\end{aligned}
$$

, since 

$$
\Large
\begin{aligned}
\left\lVert \zeta(t)^{\frac{1}{2}} {p_n}^{(t)} \chi^{(t)}\right\rVert_{\nu^{(t)}}^2 &= \int {( \zeta(t)^{\frac{1}{2}} {p_n}^{(t)} \chi^{(t)})}^2 \frac{\omega^{(t)}}{\zeta(t)(\chi^{(t)})^2} \\
&= \int (p_n^{(t)})^2 \omega^{(t)} \\
&= \left\lVert p_n^{(t)}\right\rVert_{\mu^{(t)}}^2 = 1 \quad.
\end{aligned}
$$

Therefore, $$\begin{aligned}
\langle {g_n}^{(t)}, {g_m}^{(t)} \rangle_{\nu^{(t)}} = \lambda_n^2 \delta_{n,m} \quad.
\end{aligned}$$

Note that when $$\lambda_n=\pm1$$, the basis $$ \{ {g_n}^{(t)} \} $$ is an
orthonormal basis with respect to the measure $$\nu^{(t)}$$, at every time
$$t$$. Notationally, let $${g_n}^{(t)}(x)=g_n(t, x)$$ as usual.


<br>

# Reference:
- [https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
- [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)
- [https://developer.nvidia.com/cuda-toolkit-archive](https://developer.nvidia.com/cuda-toolkit-archive)
- [https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#environment-setup](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#environment-setup)
