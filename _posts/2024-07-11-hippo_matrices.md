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
        <li><a href="#backgroud-and-setup">Background and Setup</a></li>
        <ul>
            <li><a href="#approximation">Approximation</a></li>
            <li><a href="#orthogonal-polynomials-basis">Orthogonal Polynomial Basis</a></li>
            <li><a href="#tilted-measure-and-basis">Tilted Measure and Basis</a></li>
        </ul>
        <li><a href="#the-projection-and-coefficients">The Projection and Coefficients</a></li>
        <ul>
            <li><a href="#approximate-input-function">Approximate Input Function</a></li>
            <li><a href="#reconstruct-input-function">Reconstruct Input Function</a></li>
            <li><a href="#coefficient-dynamics">Coefficient Dynamics</a></li>
        </ul>
        <li><a href="#case-hippo-legs">Case: HiPPO-LegS</a></li>
        <ul>
            <li><a href="#properties-of-legendre-polynomials">Properties of Legendre Polynomials</a></li>
            <li><a href="#shifted-and-scaled-legendre-polynomials">Shifted and Scaled Legendre Polynomials</a></li>
            <li><a href="#derivatives-of-legendre-polynomials">Derivatives of Legendre Polynomials</a></li>
            <li><a href="#derive-hippo-legs">Derive HiPPO-LegS</a></li>
        </ul>
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

## Background and Setup

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
#### Approximation

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
#### Measures

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
$$\omega(t, x)dx$$. 
That is
$$
\begin{aligned}
\int d\mu^{(t)}(x) = \int \omega(t, x)dx
\end{aligned}
$$.


<br>
#### Orthogonal Polynomial Basis

We can use a sequence of orthogonal polynomials (OPs) as our basis to approximate the continuous function $$f$$.
A sequence of OPs $$P_0(x), P_1(x), ...$$ satisfying:

1. $$\text{deg}(P_i)=i$$, and

2. $$\langle P_i, P_j \rangle = \int P_i(x) P_j(x) d\mu(x) = 0$$ for all $$i \neq j$$

A sequence of OPs $$g = \{P_n\}_{n \in \mathbb{N}}$$ of degree $$\text{deg}(g)<N$$ that approximate a function $$f$$ is
then given by

$$
\begin{aligned}
    & \sum_{i=0}^{N-1} c_i \frac{P_i(x)}{\left\lVert P_i\right\rVert^2_\mu} \quad \text{where } c_i = \langle f, P_i \rangle = \int f(x)P_i(x)d\mu(x) \\
\end{aligned}.
$$

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
#### Tilted Measure and Basis
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
\tag{eq:1}
\begin{aligned}
    {g_n}^{(t)} = \lambda_n \zeta(t)^{\frac{1}{2}} {p_n}^{(t)} \chi^{(t)}, \quad n \in \mathbb{N}
\end{aligned}
$$


Therefore, we have

$$
\Large
\label{gn_gm}
\tag{eq:2}
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
Using the result from the formula [[eq:1]](#eq:1), we have

$$
\label{approx_f}
\tag{eq:approx_f}
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
At any time $$t$$, $$f(x)_{x \leq t}$$ can be explicitly reconstructed by using $$ c_n(t) = \langle f(x)_{x \leq t}, {g_n}^{(t)} \rangle_{\nu^{(t)}} $$ and the formula [[eq:2]](#eq:2), such that

$$
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
\begin{aligned}
    \frac{d}{dt} c_n(t) &= \frac{d}{dt}( \langle f(x)_{x \leq t}, {g_n}^{(t)} \rangle_{\nu^{(t)}} ) \\
    &= \frac{d}{dt}( \zeta(t)^{-\frac{1}{2}} \lambda_n \int f \textcolor{red}{ {p_n}^{(t)} } \textcolor{green}{ \frac{\omega^{(t)}}{\chi^{(t)}} } ) \\
    &= \zeta(t)^{-\frac{1}{2}} \lambda_n \int f \textcolor{red}{ \frac{\partial}{\partial t}({p_n}^{(t)}) } \frac{\omega^{(t)}}{\chi^{(t)}} + \zeta(t)^{-\frac{1}{2}} \lambda_n \int f {p_n}^{(t)} \textcolor{green}{ \frac{\partial}{\partial t}(\frac{\omega^{(t)}}{\chi^{(t)}} ) } \\
    &= \zeta(t)^{-\frac{1}{2}} \lambda_n \int f(x) (\frac{\partial}{\partial t} p_n(t, x)) \frac{\omega}{\chi}(t, x) dx + \int f(x) (\zeta^{-\frac{1}{2}}\lambda_n  p_n(t, x)) (\frac{\partial}{\partial t} \frac{\omega}{\chi}(t, x)) dx \quad.
\end{aligned}
$$

<br>
## Case: HiPPO-LegS

<br>
#### Properties of Legendre Polynomials

An especially compact expression for the Legendre polynomials is given
by Rodrigues' formula [[4]](#4): 

$$\begin{aligned}
P_n(x) = \frac{1}{2^n n!} \frac{d^n}{dx^n}(x^2 - 1)^n .
\end{aligned}$$

The first few Legendre polynomials are: 

$$\begin{aligned}
P_0(x) &= 1 \\
P_1(x) &= x \\
P_2(x) &= \frac{1}{2} (3x^2 - 1) \\
P_3(x) &= \frac{1}{2} (5x^3 - 3x) \\
\end{aligned}$$

The Legendre polynomials are orthogonal over $$(-1,1)$$ with respect to
the measure $$ \omega^{leg}=\boldsymbol{1}_{[-1, 1]}$$ and they satisfy

$$
\label{legendre}
\tag{eq:3}
\begin{aligned}
    \frac{2n+1}{2}\int_{-1}^{1} P_n(x) P_m(x) dx = \delta_{mn}
\end{aligned}
$$

where $$\delta_{mn}$$ is Kronecker delta, equal to $$1$$ if $$m = n$$ and to $$0$$ otherwise, such that

$$\begin{aligned}
\delta_{mn} = 
    \begin{cases}
    0,& \text{if } m \neq n \\
    1,& \text{if } m = n \\
\end{cases}
\end{aligned}$$

Also, they satisfy 

$$\begin{aligned}
    P_n(1) = 1 \\
    P_n(-1) = (-1)^n \\
\end{aligned}$$

so called \"Pointwise evaluations\".


<br>
#### Shifted and Scaled Legendre polynomials

The \"shifted\" Legendre polynomials are a set of functions analogous to
the Legendre polynomials, but defined on the interval $$(0, 1)$$ [[5]](#5). They obey
the orthogonality relationship 

$$\begin{aligned}
    (2n+1)\int_{0}^{1} P_n(x) P_m(x) dx = \delta_{mn}
\end{aligned}$$

We will also consider scaling the Legendre polynomials to be orthogonal
on the interval $$[0, t]$$. Let $$u=\frac{2x}{t}-1$$, and apply a change of variables from [[eq:3]](#eq:3)

$$\begin{aligned}
    & \frac{2n+1}{2}\int_{u=-1}^{u=1} P_n(u) P_m(u) du \\
    & \text{since } u = \frac{2x}{t}-1 \text{ , we have } du = \frac{2}{t}dx  \\
    & \text{since } x = \frac{t}{2}(u+1) \text{ , we have } x = 0 \text{ when } u=-1 \text{ ,and } x=t \text{ when } u=1  \\
    & \text{replace } u \text{ with } x \\
    &= \frac{2n+1}{2}\int_{x=0}^{x=t} P_n(\frac{2x}{t}-1) P_m(\frac{2x}{t}-1) \frac{2}{t}dx \\
    &= (2n+1)\int_{x=0}^{x=t} P_n(\frac{2x}{t}-1) P_m(\frac{2x}{t}-1) \frac{1}{t}dx \\
    &= (2n+1) \int_{x=0}^{x=t} P_n(\frac{2x}{t}-1) P_m(\frac{2x}{t}-1) \frac{1}{2}\omega^{leg}(\frac{2x}{t}-1) \frac{1}{t} dx \\
    % &= \frac{(2n+1)}{2} \int_{x=0}^{x=t} P_n(\frac{2x}{t}-1) P_m(\frac{2x}{t}-1) \omega^{leg}(\frac{2x}{t}-1) \frac{1}{t} dx \\
    &= \delta_{nm}
\end{aligned}$$

Therefore, with respect to the measure $$\omega_t=\boldsymbol{1}_{[0, t]} / t$$ (which is a probability measure for all t), we have

$$\begin{aligned}
    & (2n+1) \int_{x=0}^{x=t} P_n(\frac{2x}{t}-1) P_m(\frac{2x}{t}-1) \frac{1}{2}\omega^{leg}(\frac{2x}{t}-1) \frac{1}{t} dx \\
    &= (2n+1) \int_{x=0}^{x=t} P_n(\frac{2x}{t}-1) P_m(\frac{2x}{t}-1) \omega_{t}(\frac{2x}{t}-1) dx \\
\end{aligned}$$

Then, the normalized orthogonal polynomials in the interval $$[0, t]$$ with starting point is $$\textcolor{red}{0}$$ and step size $$\textcolor{green}{t}$$ are

$$\begin{aligned}
    (2n+1)^{\frac{1}{2}} P_n(\frac{2x}{t}-1).
    \quad \left( =(2n+1)^{\frac{1}{2}} P_n(\frac{2(x-\textcolor{red}{0})}{\textcolor{green}{t}}-1) \right)
\end{aligned}$$

Generalize to any interval $$[t-\theta, t]$$, we use $$\textcolor{red}{t-\theta}$$ as the starting point and $$\textcolor{green}{\theta}$$ as the step size. After replacing the notation, we have

$$\begin{aligned}
    &(2n+1)^{\frac{1}{2}} P_n(\frac{2(x - \textcolor{red}{ (t-\theta)}) }{\textcolor{green}{\theta}}-1) \\
    &=(2n+1)^{\frac{1}{2}} P_n(\frac{2x-2t+2\theta}{\theta} - 1) \\
    &=(2n+1)^{\frac{1}{2}} P_n(\frac{2(x-t)}{\theta} + 1) \\
\end{aligned}$$

, which is orthonormal for the uniform measure
$$\omega_\theta= \frac{1}{ \theta}\boldsymbol{1}_{[t-\theta, t]}$$

<br>
#### Derivatives of Legendre polynomials

use the following recurrence relations (the integration of Legendre polynomial [[4]](#4))

$$
\label{legendre_recur_1}
\tag{eq:4}
\begin{aligned}
    (2n+1)P_n &= P'_{n+1} - P'_{n-1}
\end{aligned}
$$ 

and

$$
\label{legendre_recur_2}
\tag{eq:5}
\begin{aligned}
    P'_{n+1} &= (n+1)P_n + xP'_n
\end{aligned}
$$

The first equation [[eq:4]](#eq:4) yields 

$$\begin{aligned}[c]
    P'_{n+1} &= (2\mathbf{n}+1)P_\mathbf{n} + \textcolor{red}{P'_{n-1}} \\
    \textcolor{red}{P'_{n-1}} &= (2(\mathbf{n-2})+1)P_\mathbf{n-2} + \textcolor{green}{P'_{n-3}} \\
    \textcolor{green}{P'_{n-3}} &= (2(\mathbf{n-4})+1)P_\mathbf{n-4} + \textcolor{blue}{P'_{n-5}} \\
    ...
\end{aligned}
\quad\quad
\begin{aligned}[c]
    P'_{n} &= (2(\mathbf{n-1})+1)P_\mathbf{n-1} + \textcolor{red}{P'_{n-2}} \\
    \textcolor{red}{P'_{n-2}} &= (2(\mathbf{n-3})+1)P_\mathbf{n-3} + \textcolor{green}{P'_{n-4}} \\
    \textcolor{green}{P'_{n-4}} &= (2(\mathbf{n-5})+1)P_\mathbf{n-5} + \textcolor{blue}{P'_{n-6}} \\
    ...
\end{aligned}
$$

Therefore we have

$$\begin{aligned}
    & P'_{n+1} = (2n+1)P_n + (2n-3)P_{n-2} + (2n-7)P_{n-4} ... \quad \text{ and} \\
    & P'_n = (2n-1)P_{n-1} + (2n-5)P_{n-3} + (2n-9)P_{n-5} ...
\end{aligned}$$

where the sum stops at $$P_0$$ or $$P_1$$. By summing $$P'_{n+1} + P'_n $$, we have

$$\begin{aligned}
    & P'_{n+1} +  P'_n = \textcolor{red}{(2n+1)P_n} + (2n-1)P_{n-1}+ (2n-3)P_{n-2} + (2n-5)P_{n-3} + (2n-7)P_{n-4} ... \\
    & P'_{n+1} +  P'_n = \textcolor{red}{nP_n + (n+1)P_n} + (2n-1)P_{n-1}+ (2n-3)P_{n-2} + (2n-5)P_{n-3} + (2n-7)P_{n-4} ... \text{ (using eq:5)}\\
    & P'_{n+1} +  P'_n = nP_n + \textcolor{green}{P'_{n+1} - xP'_n} + (2n-1)P_{n-1}+ (2n-3)P_{n-2} + (2n-5)P_{n-3} + (2n-7)P_{n-4} ... \\
    &  P'_n \textcolor{green}{+ xP'_n} = nP_n + (2n-1)P_{n-1}+ (2n-3)P_{n-2} + (2n-5)P_{n-3} + (2n-7)P_{n-4} ... \\
\end{aligned}
$$


Therefore, we have 

$$
\label{legendre_result}
\tag{eq:6}
\begin{aligned}
    (x+1)P'_n(x) &= nP_n + (2n-1)P_{n-1}+ (2n-3)P_{n-2} + (2n-5)P_{n-3} + (2n-7)P_{n-4} ... \\
\end{aligned}
$$

We will use the result [[eq:6]](#eq:6) to derive HiPPO-LegS.

<br>
#### Derive HiPPO-LegS

Now we have the measure $$\omega(t, x) = \frac{1}{t}\mathbb{1}_{[0,t]}$$
and the basis 

$$g_n(t, x) = p_n(t, x) = (2n+1)^{\frac{1}{2}}P_n(\frac{2x}{t}-1)$$

where $$P_n$$ are the basic Legendre polynomials.



Since the basis functions $$g_n(t, x)$$ are orthonormal with respect to the measure, we have the tiling function
$$\chi(t, x) = 1$$ (no tilting), $$\zeta(t, x) = 1$$, $$\lambda_n=1$$

Pug $$\chi, \zeta, \lambda_n$$ into [[eq:approx_f]](#eq:approx_f), we have

$$\begin{aligned}
    c_n(t) &= \zeta(t)^{-\frac{1}{2}} \lambda_n \int f {p_n}^{(t)}\frac{\omega^{(t)}}{\chi^{(t)}} =  \int f {p_n}^{(t)} \omega^{(t)}
\end{aligned}$$ 


- **HiPPO-LegS Derivatives**

We first differentiate the measure and basis:

$$\begin{aligned}
    \frac{\partial}{\partial t} \omega(t, \cdot) &= -t^{-2} \mathbb{1}_{[0, t]} + t^{-1} \delta_t = t^{-1} (-\omega(t) + \delta_t) \\
    \frac{\partial}{\partial t} g_n(t, x) &= -(2n+1)^{\frac{1}{2}}2xt^{-2} {P'}_n(\frac{2x}{t}-1) \\
    &= -(2n+1)^{\frac{1}{2}}t^{-1} (\textcolor{red}{\frac{2x}{t}-1}+1) {P'}_n(\textcolor{red}{\frac{2x}{t}-1}) \quad.
\end{aligned}$$ 

Now define $$z = \frac{2x}{t} - 1$$ and apply the
result of derivatives of Legendre polynomials in the equation [[eq:6]](#eq:6). 

$$\begin{aligned}
    \frac{\partial}{\partial t} g_n(t, x) &= -(2n+1)^{\frac{1}{2}}t^{-1} (z+1) {P'}_n(z) \\
    &= -(2n+1)^{\frac{1}{2}}t^{-1} [nP_n(z) + (2n-1)P_{n-1}(z) + (2n-3)P_{n-2}(z) + ...] \\
    & \left( \text{using } \quad (2n+1)^{-\frac{1}{2}} g_n(t, x) = P_n(\frac{2x}{t}-1) = P_n(z)\right) \\
    &= -t^{-1}(2n+1)^{\frac{1}{2}} [n (2n+1)^{-\frac{1}{2}} g_n(t, x) + (2n-1)^{\frac{1}{2}} g_{n-1}(t, x) + (2n-3)^{\frac{1}{2}}g_{n-2}(t, x) + ...] \\
    &= -t^{-1}(2n+1)^{\frac{1}{2}} [n (2n+1)^{-\frac{1}{2}} g_n^{(t)} + (2n-1)^{\frac{1}{2}} g_{n-1}^{(t)} + (2n-3)^{\frac{1}{2}}g_{n-2}^{(t)} + ...] \\
\end{aligned}$$

- **HiPPO-LegS Coefficient Dynamics**

$$\begin{aligned}
    \frac{d}{dt} c_n(t) &=  \frac{d}{dt} ( \int f {p_n}^{(t)} \omega^{(t)} dx) \\
    &= \int f (\textcolor{red}{\frac{\partial}{\partial t} g_n^{(t)}}) \omega^{(t)} dx  + \int f g_n^{(t)} \textcolor{green}{\frac{\partial}{\partial t} \omega^{(t)}} dx \\
    
    &= \int f \left( \textcolor{red}{ -t^{-1}(2n+1)^{\frac{1}{2}} \left[ n (2n+1)^{-\frac{1}{2}}   g_n^{(t)}  + (2n-1)^{\frac{1}{2}} g_{n-1}^{(t)} + ...\right] } \right) \omega^{(t)} dx
    + \left( \int f g_n^{(t)} \textcolor{green}{t^{-1} (-\omega^{(t)} + \delta_t)} dx \right)  \\

    &= \left( -t^{-1}(2n+1)^{\frac{1}{2}} \left[n (2n+1)^{-\frac{1}{2}} \textcolor{red}{\int f g_n^{(t)} \omega^{(t)} dx}  + (2n-1)^{\frac{1}{2}} \textcolor{red}{\int f g_{n-1}^{(t)} \omega^{(t)} dx}  + ...\right] \right)
    + \left( \int f g_n^{(t)} t^{-1} (-\omega^{(t)} + \delta_t) dx \right)   \\
    &= -t^{-1}(2n+1)^{\frac{1}{2}} [n (2n+1)^{-\frac{1}{2}} \textcolor{red}{c_n(t)} + (2n-1)^{\frac{1}{2}} \textcolor{red}{c_{n-1}(t)} + (2n-3)^{\frac{1}{2}}  \textcolor{red}{c_{n-2}(t)} + ...]
    + \left( \int f g_n^{(t)} t^{-1} (-\omega^{(t)} + \delta_t) dx \right)   \\
    &= -t^{-1}(2n+1)^{\frac{1}{2}} [n (2n+1)^{-\frac{1}{2}} c_n(t) + (2n-1)^{\frac{1}{2}} c_{n-1}(t) + (2n-3)^{\frac{1}{2}}  c_{n-2}(t) + ...]
     + \left( - t^{-1} \textcolor{green}{ \int f g_n^{(t)} \omega^{(t)}dx} +   t^{-1} \textcolor{green}{ \int f g_n^{(t)} \delta_t dx} \right)   \\
    &= -t^{-1}(2n+1)^{\frac{1}{2}} [n (2n+1)^{-\frac{1}{2}} c_n(t) + (2n-1)^{\frac{1}{2}} c_{n-1}(t) + (2n-3)^{\frac{1}{2}}  c_{n-2}(t) + ...]
    -t^{-1} \textcolor{green}{c_n(t)} + t^{-1} \textcolor{green}{f(t)  g_{n}^{(t)}(t)}  \\
    &= -t^{-1}(2n+1)^{\frac{1}{2}} [n (2n+1)^{-\frac{1}{2}} c_n(t) + (2n-1)^{\frac{1}{2}} c_{n-1}(t) + (2n-3)^{\frac{1}{2}}  c_{n-2}(t) + ...]
    -t^{-1} c_n(t) + t^{-1} \textcolor{green}{f(t)  (2n+1)^{\frac{1}{2}}P_n(1)}  \\
    &= -t^{-1}(2n+1)^{\frac{1}{2}} [n (2n+1)^{-\frac{1}{2}} c_n(t) + (2n-1)^{\frac{1}{2}} c_{n-1}(t) + (2n-3)^{\frac{1}{2}}  c_{n-2}(t) + ...]
    \textcolor{red}{-t^{-1}c_n(t)} + t^{-1} f(t) (2n+1)^{\frac{1}{2}}   \\
    &= -t^{-1}(2n+1)^{\frac{1}{2}} [\textcolor{red}{(n+1)} (2n+1)^{-\frac{1}{2}} c_n(t) + (2n-1)^{\frac{1}{2}} c_{n-1}(t) + (2n-3)^{\frac{1}{2}}  c_{n-2}(t) + ...]
    + t^{-1} f(t) (2n+1)^{\frac{1}{2}}   \\
\end{aligned}$$ 

For example: 

$$\begin{aligned}
    \frac{d}{dt} c_0(t) &= -\frac{1}{t} [(n+1) c_0(t)] = -\frac{1}{t} [1 c_0(t)] \\
    \frac{d}{dt} c_1(t) &= -\frac{1}{t} [(n+1) c_1(t) + (2n+1)^{\frac{1}{2}}(2n-1)^{\frac{1}{2}}c_0(t)] = -\frac{1}{t} [2 c_1(t) + 3^{\frac{1}{2}}1^{\frac{1}{2}}c_0] \\
    \frac{d}{dt} c_2(t) &= -\frac{1}{t} [(n+1) c_2(t) + (2n+1)^{\frac{1}{2}}(2n-1)^{\frac{1}{2}}c_1(t)+ (2n+1)^{\frac{1}{2}}(2n-3)^{\frac{1}{2}}c_0(t)] \\ 
    & \quad = -\frac{1}{t} [3 c_2(t) + 5^{\frac{1}{2}}3^{\frac{1}{2}}c_1+ 5^{\frac{1}{2}}1^{\frac{1}{2}}c_0] \\
    & ... \\
    \frac{d}{dt} c_n(t) &= -\frac{1}{t} [(n+1) c_n(t) + \sum_{k=n-1}^{0}(2n+1)^{\frac{1}{2}}(2k+1)^{\frac{1}{2}}c_k(t)] \\ 
\end{aligned}$$

Writing the result into the matrix form: 

$$\begin{aligned}
    \frac{d}{dt} c_n(t) = -\frac{1}{t} A c(t) + \frac{1}{t} B f(t)
\end{aligned}$$

where 

$$\begin{aligned}
A_{nk} &= 
\begin{cases}
    (2n+1)^{\frac{1}{2}}(2k+1)^{\frac{1}{2}},& \text{if } n > k \\
    n+1,& \text{if } n = k \\
    0,& \text{if } n < k \\
\end{cases}
\\
B &= (2n+1)^{\frac{1}{2}}
\end{aligned}$$

We can also write the result into the form: $$\begin{aligned}
    \frac{d}{dt} c_n(t) = -\frac{1}{t} D[ MD^{-1}c(t) + \mathbb{1} f(t)]
\end{aligned}$$

where 

$$\begin{aligned}
D &= \text{diag}[(2n+1)^{\frac{1}{2}}]_0^{n-1} \\
M_{nk} &= 
\begin{cases}
    2k+1,& \text{if } n > k \\
    k+1,& \text{if } n = k \\
    0,& \text{if } n < k \\
\end{cases}
\end{aligned}$$

- **HiPPO-LegS Reconstruction**

$$\begin{aligned}
    f(x)_{x \leq t} \approx {g}^{(t)} &= \sum_{n=0}^{N-1} \lambda_n^{-1} \zeta^{\frac{1}{2}} c_n(t) {p_n}^{(t)} \chi^{(t)} \\
    &= \sum_{n=0}^{N-1} c_n(t) {p_n}^{(t)} = \sum_{n=0}^{N-1} c_n(t) {g_n}(t, x)\\
    &= \sum_{n=0}^{N-1} c_n(t) (2n+1)^{\frac{1}{2}}P_n(\frac{2x}{t}-1)
\end{aligned}$$

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

<a id="4">[4]</a> 
<a href="https://en.wikipedia.org/wiki/Legendre_polynomials">https://en.wikipedia.org/wiki/Legendre_polynomials</a> 


<a id="5">[5]</a> 
<a href="https://mathworld.wolfram.com/LegendrePolynomial.html">https://mathworld.wolfram.com/LegendrePolynomial.html</a> 