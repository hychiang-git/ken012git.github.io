---
layout: post
title: "HiPPO Matrices"
date: 2024-07-11
description: HiPPO Matrices
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
  <li><a href="#derive-hippo-matrices">Derive HiPPO Matrices</a></li>
    <ul>
        <li><a href="#setup">Setup</a></li>
        <li><a href="#approximation">Approximation</a></li>
        <li><a href="#orthogonal-polynomials-basis">Orthogonal Polynomial Basis</a></li>
        <li><a href="#tilted-measure-and-basis">Tilted Measure and Basis</a></li>
        <li><a href="#the-projection-and-coefficients">The Projection and Coefficients</a></li>
        <ul>
            <li><a href="#approximate-input-function">Approximate Input Function</a></li>
            <li><a href="#reconstruct-input-function">Reconstruct Input Function</a></li>
            <li><a href="#coefficient-dynamics">Coefficient Dynamics</a></li>
        </ul>
        <li><a href="#derive-hippo-legs-matrices">Derive HiPPO-LegS Matrices</a></li>
    </ul>
  <li><a href="#references">References</a></li>
</ul>
</details>

<br>

# Introduction
This document provides the mathematic derivations of the HiPPO matrices that help readers understand the formulas. The contents are extracted and summarized from the original papers cited in <a href="#references">References</a>.

<br>

# Derive HiPPO Matrices

The contents are extracted and summarized from the original paper [[1]](#1), but we show more details in the derivations to help people to understand.

<br>

## Setup

HiPPO considers a linear time-invariant (LTI) ODEs such that

$$
\Large
\begin{aligned}
\dot{c}(t)=Ac(t)+Bf(t) .
\end{aligned}
$$

1.  $$c(t)$$ is the coefficient vector that describes the combination of
    the basis functions, which corresponds to the state vector $$h(t)$$ in the continuous state space
    model.

2.  $$f(t)$$ is the input signal that corresponds to the input vector $$x(t)$$ in the continuous state
    space model.
    
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

Define $$\nu$$ to be the `normalized measure` with density proportional to
$$\omega^{t}/(\chi^{(t)})^2$$.
We will calculate the normalized measure and the orthonormal basis for it.

Let $$\zeta(t)$$ to be the normalization
constant: $$\begin{aligned}
    \zeta(t) = \int \frac{\omega}{\chi^2} = \int \frac{\omega^{(t)}(x)}{({\chi^{(t)}(x)})^2}dx
\end{aligned}$$ , so that $$\nu^{(t)}$$ has density
$$\frac{\omega^{(t)}(x)}{\zeta(t)(\chi^{(t)}(x))^2}$$
If $$\chi(t, x) = 1$$ (no tilting), this constant is $$\chi(t) = 1$$.

Note that (dropping the dependence on x inside the integral for shorthand)

$$
\Large
\begin{aligned}
\left\lVert \zeta(t)^{\frac{1}{2}} {p_n}^{(t)} \chi^{(t)}\right\rVert_{\nu^{(t)}}^2 &= \int {( \zeta(t)^{\frac{1}{2}} {p_n}^{(t)} \chi^{(t)})}^2 \frac{\omega^{(t)}}{\zeta(t)(\chi^{(t)})^2} \\
&= \int (p_n^{(t)})^2 \omega^{(t)} \\
&= \left\lVert p_n^{(t)}\right\rVert_{\mu^{(t)}}^2 = 1 \quad.
\end{aligned}
$$

Thus we define the `orthogonal basis` for $$\nu^{(t)}$$ (normalized measurement)

$$
\Large
\label{orthogonal_basis}
\tag{1}
\begin{aligned}
    {g_n}^{(t)} = \lambda_n \zeta(t)^{\frac{1}{2}} {p_n}^{(t)} \chi^{(t)}, \quad n \in \mathbb{N}
\end{aligned}
$$


Therefore, we have

$$
\Large
\label{gn_gm}
\tag{2}
\begin{aligned}
\langle {g_n}^{(t)}, {g_m}^{(t)} \rangle_{\nu^{(t)}} = \lambda_n^2 \delta_{n,m} \quad.
\end{aligned}
$$

Note that when $$\lambda_n=\pm1$$, the basis $$ \{ {g_n}^{(t)} \} $$ is an
orthonormal basis with respect to the measure $$\nu^{(t)}$$, at every time
$$t$$. Notationally, let $${g_n}^{(t)}(x)=g_n(t, x)$$ as usual.

<br>

## The Projection and Coefficients

Given a choice of measures $$\nu$$ and basis functions $$g$$, we compute the
coefficients $$c(t)$$ to approximate a $$C^1$$-smooth function which is seen
*online* $$f(x)_{x \leq t}$$.

<br>

#### Approximate Input Function
We project the input function $$f$$ to basis functions $$g$$, and $$c(t)$$ is the coefficient vector that describes the magnitude of the basis functions.
Using the result from the formula [[1]](#1), we have

$$
\Large
\begin{aligned}
    c_n(t) &= \langle f(x)_{x \leq t}, {g_n}^{(t)} \rangle_{\nu^{(t)}} \\
    &= \int f \textcolor{red}{ {g_n}^{(t)} }  \frac{\omega^{(t)}}{\zeta(t)(\chi^{(t)})^2} \\
    &= \int f \textcolor{red} { \lambda_n \zeta(t)^{\frac{1}{2}} {p_n}^{(t)} \chi^{(t)} } \frac{\omega^{(t)}}{\zeta(t)(\chi^{(t)})^2} \\
    &= \int f \lambda_n \zeta(t)^{-\frac{1}{2}} {p_n}^{(t)}\frac{\omega^{(t)}}{\chi^{(t)}} \\
    &= \zeta(t)^{-\frac{1}{2}} \lambda_n \int f {p_n}^{(t)}\frac{\omega^{(t)}}{\chi^{(t)}}
\end{aligned}
$$

<br>

#### Reconstruct Input Function
The function $$f$$ can be approximated by storing its coefficients with respect to the basis.
At any time $$t$$, $$f(x)_{x \leq t}$$ can be explicitly reconstructed by using $$ c_n(t) = \langle f(x)_{x \leq t}, {g_n}^{(t)} \rangle_{\nu^{(t)}} $$ and the formula [[2]](#2), such that

$$
\Large
\begin{aligned}
    f(x)_{x \leq t} \approx {g}^{(t)} &= \sum_{n=0}^{N-1} \textcolor{red} { \langle f(x)_{x \leq t}, {g_n}^{(t)} \rangle_{\nu^{(t)}} } \frac{ {g_n}^{(t)} }{ {\left \lVert {g_n}^{(t)} \right \rVert }_{\nu^{(t)}}^2} \\
    &= \sum_{n=0}^{N-1} \textcolor{red}{c_n(t)} \frac{ {g_n}^{(t)} }{ \textcolor{green}{ {\left \lVert {g_n}^{(t)} \right \rVert }_{\nu^{(t)}}^2} } \\
    &= \sum_{n=0}^{N-1} c_n(t) \frac{ {g_n}^{(t)} }{ \textcolor{green}{ \langle {g_n}^{(t)}, {g_n}^{(t)} \rangle_{\nu^{(t)}} } } \\
    &= \sum_{n=0}^{N-1} c_n(t) \frac{ {g_n}^{(t)} }{ \textcolor{green}{\lambda_n^2} } \\
    &= \sum_{n=0}^{N-1} \lambda_n^{-2} c_n(t) {g_n}^{(t)} \\
    &= \sum_{n=0}^{N-1} \lambda_n^{-1} \zeta^{\frac{1}{2}} c_n(t) {p_n}^{(t)} \chi^{(t)} \\
\end{aligned}
$$

<br>

#### Coefficient Dynamics

The coefficients $$c(t)$$ encode information about the history of $$f$$ and
allow online predictions. We compute these coefficients over time by
viewing them as a dynamical system. Differentiating $$c_n(t)$$, we have

$$
\Large
\begin{aligned}
    \frac{d}{dt} c_n(t) &= \frac{d}{dt}( \langle f(x)_{x \leq t}, {g_n}^{(t)} \rangle_{\nu^{(t)}} ) \\
    &= \frac{d}{dt}( \zeta(t)^{-\frac{1}{2}} \lambda_n \int f \textcolor{red}{ {p_n}^{(t)} } \textcolor{green}{ \frac{\omega^{(t)}}{\chi^{(t)}} } ) \\
    &= \zeta(t)^{-\frac{1}{2}} \lambda_n \int f \textcolor{red}{ \frac{\partial}{\partial t}({p_n}^{(t)}) } \frac{\omega^{(t)}}{\chi^{(t)}} + \zeta(t)^{-\frac{1}{2}} \lambda_n \int f {p_n}^{(t)} \textcolor{green}{ \frac{\partial}{\partial t}(\frac{\omega^{(t)}}{\chi^{(t)}} ) } \\
    &= \zeta(t)^{-\frac{1}{2}} \lambda_n \int f(x) (\frac{\partial}{\partial t} p_n(t, x)) \frac{\omega}{\chi}(t, x) dx + \int f(x) (\zeta^{-\frac{1}{2}}\lambda_n  p_n(t, x)) (\frac{\partial}{\partial t} \frac{\omega}{\chi}(t, x)) dx \quad.
\end{aligned}
$$

<br>
## Derive HiPPO-LegS Matrices

From paper [@gu2020hippo]

<br>

# References


<a id="1">[1]</a> 
A. Gu, T. Dao, S. Ermon, A. Rudra, and C. Ré, “Hippo: Recurrent memory with optimal
polynomial projections,” Advances in neural information processing systems, vol. 33, pp. 1474–
1487, 2020.

<a id="2">[2]</a> 
A. Gu, K. Goel, A. Gupta, and C. Ré, “On the parameterization and initialization of diagonal state
space models,” Advances in Neural Information Processing Systems, vol. 35, pp. 35971–35983,
2022.

<a id="3">[3]</a> 
A. Gu and T. Dao, “Mamba: Linear-time sequence modeling with selective state spaces,” arXiv
preprint arXiv:2312.00752, 2023.
