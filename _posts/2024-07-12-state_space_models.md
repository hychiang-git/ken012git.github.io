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
  <li><a href="#references">References</a></li>
</ul>
</details>

<br>

# Introduction
This document provides the mathematic derivations of the HiPPO matrices that help readers understand the formulas. The contents are extracted and summarized from the original papers cited in <a href="#references">References</a>.

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

# Discretize Continuous State Space Model

The general formulation of an ODE is
$$\begin{aligned}
    \frac{d}{dt}c(t) = f(t, c(t))
\end{aligned}$$
where
$$\begin{aligned}
    \frac{d}{dt}c(t) = A c(t) + Bf(t).
\end{aligned}
$$

From the differential equation, we have $$\begin{aligned}
    c(t+\Delta t) - c(t) = \int_{t}^{t+\Delta t} f(s, c(s))ds .
\end{aligned}$$

## Bilinear Transform

By the definition of the integral rule, we have 

$$
\begin{aligned}
    c(t+\Delta t) - c(t) &= \Delta t \frac{f(t, c(t)) - f(t+\Delta t, c(t+\Delta t))}{2} \\
    &= \frac{\Delta t}{2} (Ac(t) + Bf(t) + Ac(t+\Delta t) + Bf(t+\Delta t)) \\
    &= \frac{\Delta t}{2} Ac(t) + \frac{\Delta t}{2}Bf(t) + \frac{\Delta t}{2}Ac(t+\Delta t) + \frac{\Delta t}{2}Bf(t+\Delta t)
\end{aligned}
$$

By combining the terms, we have 

$$
\begin{aligned}
    c(t+\Delta t) - \frac{\Delta t}{2}Ac(t+\Delta t) &= c(t) + \frac{\Delta t}{2} Ac(t) + \frac{\Delta t}{2}Bf(t) +  + \frac{\Delta t}{2}Bf(t+\Delta t) \\
    ( \mathbf{I} - \frac{\Delta t}{2}A)c(t+\Delta t) &= ( \mathbf{I}+ \frac{\Delta t}{2} A)c(t)  + \frac{\Delta t}{2} B(f(t) +  + f(t+\Delta t)) \\
    c(t+\Delta t) &= ( \mathbf{I} - \frac{\Delta t}{2}A)^{-1} ( \mathbf{I}+ \frac{\Delta t}{2} A) c(t) + \frac{\Delta t}{2} ( \mathbf{I} - \frac{\Delta t}{2}A)^{-1} B (f(t) +  f(t+\Delta t))
\end{aligned}
$$

Since $$f$$ is the constant and does no involve in the
integral, we have $$f(t) = f(t+\Delta t)$$ 

$$\begin{aligned}
    c(t+\Delta t) &= ( \mathbf{I} - \frac{\Delta t}{2}A)^{-1} ( \mathbf{I}+ \frac{\Delta t}{2} A) c(t) + \Delta t ( \mathbf{I} - \frac{\Delta t}{2} A)^{-1} B f(t+\Delta t)
\end{aligned}
$$

We can replace $$\frac{1}{2}$$ with a variable $$\alpha$$, so that we have
Generalized Bilinear Transformation (GBT). 

$$\begin{aligned}
    c(t+\Delta t) &= ( \mathbf{I} - \Delta t \alpha A)^{-1} ( \mathbf{I}+ \Delta t \alpha A) c(t) + \Delta t ( \mathbf{I} - \Delta t \alpha A)^{-1} B f(t+\Delta t)
\end{aligned}$$

We can write the discretized formula from the result with discretized
matrices $$\overline{A}, \overline{B}$$ 

$$\begin{aligned}
    c[t+1]  &= \overline{A} c[t] + \overline{B} f[t+1] \\
    \overline{A} &= (\mathbf{I} - \Delta t \alpha A)^{-1} ( \mathbf{I}+ \Delta t \alpha A) \\
    \overline{B} &= \Delta t ( \mathbf{I} - \Delta t \alpha A)^{-1} B
\end{aligned}$$

<br>
## Zero-order Hold Transform (ZOH)

#### State solution

We derive the state solution from the continuous time-invariant dynamic equation 

$$\begin{aligned}
    \frac{d}{dt}c(t) &= A c(t) + Bf(t) \\
    \frac{d}{dt}c(t) &- A c(t) =  Bf(t) .
\end{aligned}$$ 

Using the fact $$\begin{aligned}
    \frac{d}{dt}[e^{-At}c(t)] = e^{-At} \frac{d}{dt}c(t) - e^{-At} A c(t) = e^{-At} [ \frac{d}{dt}c(t) - A c(t) ]
\end{aligned}$$ , we have 

$$\begin{aligned}
    \frac{d}{dt}[e^{-At}c(t)] = e^{-At} Bf(t) \\
\end{aligned}$$ 

We integrate the both side from $$0$$ to $$t$$

$$\begin{aligned}
    \int_0^t \frac{d}{d \tau}[e^{-A\tau}c(\tau)] d\tau = e^{-At}c(t) - c(0) = \int_0^t e^{-A\tau} Bf(\tau) d \tau
\end{aligned}$$ 

After multiplying $$e^{At}$$ on both sides, we have the
state solution 

$$\begin{aligned}
    c(t) = e^{At}c(0) + \int_0^t e^{A(t-\tau)} Bf(\tau) d \tau
\end{aligned}$$ 

Note that $$\int_0^t e^{A(t-\tau)} Bf(\tau) d \tau$$ is a
convolution, which can be done efficiently by multiplying the scalars in
the frequency domain. We will use this result to derive the Zero-order
Hold discretization.

<br>
#### Zero-order Hold Discretization

We first define the relationship between discretized signals and
continuous signals. 

$$\begin{aligned}
    c[k] := c(k\Delta t), \quad \text{and} \quad f[k] := f(k\Delta t)
\end{aligned}$$

where $$c[x]$$ and $$f[k]$$ is a zero-order transformed
signals from $$c(x)$$ and $$f(x)$$ with a sampling step $$k$$.

Then, we have 

$$\begin{aligned}
    c[k+1] &:= c((k+1)\Delta t) \\
    &= e^{A(k+1)\Delta t}c(0) + \int_0^{(k+1)\Delta t} e^{A((k+1)\Delta t-\tau)} Bf(\tau) d \tau \\
    &= e^{A(k+1)\Delta t}c(0) + \int_0^{k\Delta t} e^{A((k+1)\Delta t-\tau)} Bf(\tau) d \tau + \int_{k\Delta t}^{(k+1)\Delta t} e^{A((k+1)\Delta t-\tau)} Bf(\tau) d \tau \\
    &= e^{A\Delta t}e^{Ak\Delta t}c(0) + \int_0^{k\Delta t} e^{A\Delta t}e^{A(k\Delta t-\tau)} Bf(\tau) d \tau + \int_{k\Delta t}^{(k+1)\Delta t} e^{A((k+1)\Delta t-\tau)} Bf(\tau) d \tau \\
    &= e^{A\Delta t}\left(e^{Ak\Delta t}c(0) + \int_0^{k\Delta t} e^{A(k\Delta t-\tau)} Bf(\tau) d \tau \right) + \int_{k\Delta t}^{(k+1)\Delta t} e^{A((k+1)\Delta t-\tau)} Bf(\tau) d \tau \\
    &= e^{A\Delta t} c(k\Delta t) + \int_{k\Delta t}^{(k+1)\Delta t} e^{A((k+1)\Delta t-\tau)} Bf(\tau) d \tau \\
    &= e^{A\Delta t} c[k] + e^{A((k+1)\Delta t} B \int_{k\Delta t}^{(k+1)\Delta t} e^{-A\tau} f(\tau) d \tau \\
    &= e^{A\Delta t} c[k] + e^{A((k+1)\Delta t} B (-A^{-1}e^{-A\tau}\Big|_{k\Delta t}^{(k+1)\Delta t})f((k+1)\Delta t) \\
    &= e^{A\Delta t} c[k] + e^{A((k+1)\Delta t} B (-A^{-1}e^{-A(k+1)\Delta t} + A^{-1}e^{-A k\Delta t}) f((k+1)\Delta t) \\
&= e^{A\Delta t} c[k] + BA^{-1} (-\mathbf{I} + e^{A \Delta t}) f[k+1] \\
&= e^{A\Delta t} c[k] + \frac{\Delta t B}{\Delta t 
 A} (e^{A \Delta t} - \mathbf{I}) f[k+1] \\
\end{aligned}$$

Note that $$f$$ is constant from $$k\Delta t$$ to $$(k+1)\Delta t$$ and equal
to $$f(k\Delta t) = f((k+1)\Delta t)$$

We can write the discretized formula from the result with discretized
matrices $$\overline{A}, \overline{B}$$ 

$$\begin{aligned}
    c[t+1]  &= \overline{A} c[t] + \overline{B} f[t+1] \\
    \overline{A} &= e^{A\Delta t} \\
    \overline{B} &= {(\Delta t 
 A)}^{-1} (e^{A \Delta t} - \mathbf{I}) \Delta t B
\end{aligned}$$

# Discrete-time State Space model

We can write the continuous-time state space model 

$$\begin{aligned}
\dot{x}(t)=Ax(t)+Bu(t) \\ 
y(t)=Cx(t)+Du(t)
\end{aligned}$$ 

as a discrete-time state space model 

$$\begin{aligned}
    x[t]  &= \overline{A} x[t-1] + \overline{B} u[t] \\
    y[t]  &= C x[t] + D u[t] \\
\end{aligned}$$ 

by using the GBL or ZOH 

$$\begin{aligned}[c]
&\text{Bilinear:}\\
\overline{A}&= (\mathbf{I} - \Delta t \alpha A)^{-1} ( \mathbf{I}+ \Delta t \alpha A)\\
\overline{B} &= \Delta t ( \mathbf{I} - \Delta t \alpha A)^{-1} B
\end{aligned}
\quad\quad
\begin{aligned}[c]
&\text{ZOH:}\\
\overline{A} &= e^{A\Delta t}\\
\overline{B} &= {(\Delta t A)}^{-1} (e^{A \Delta t} - \mathbf{I}) \Delta t B\\
\end{aligned}$$

<br>
## Convolution Form

Letting $$x_{-1} = 0$$ and unrolling the $$y_k$$ 

$$\begin{aligned}[c]
&\text{t=0}\\
x_0 &= \overline{B}u_0\\
y_0 &= C\overline{B}u_0 + Du_0\\
\end{aligned}
\quad
\begin{aligned}[c]
&\text{t=1}\\
x_1 &= \overline{A}\overline{B}u_0 + \overline{B}u_1\\
y_1 &= C\overline{A}\overline{B}u_0 + C\overline{B}u_1 + Du_1\\
\end{aligned}
\quad
\begin{aligned}[c]
&\text{t=2}\\
x_2 &= \overline{A}^2\overline{B}u_0 + \overline{A}\overline{B}u_1 + \overline{B}u_2\\
y_2 &= C\overline{A}^2\overline{B}u_0 + C\overline{A}\overline{B}u_1 + C\overline{B}u_2 + Du_2\\
\end{aligned}$$ 

Therefore, when $$t=k$$ 

$$\begin{aligned}
x_k &= \overline{A}^k\overline{B}u_0 +...+ \overline{A}\overline{B}u_1 + \overline{B}u_2\\
y_k &= C\overline{A}^k\overline{B}u_0 + C\overline{A}^{k-1}\overline{B}u_1 +...+ C\overline{B}u_k + Du_k \\
\end{aligned}$$

In other words, equation (4) is a single (non-circular) convolution

$$\begin{aligned}
y_t = \sum_{\tau=0}^k \overline{C}\overline{A}^{\tau}\overline{B} u_{t-\tau}  + Du_t
\end{aligned}$$

which can be computed very efficiently with FFTs,
provided that K is known. 

$$\begin{aligned}
    y_t = \overline{K} \ast u + Du_t
\end{aligned}$$
where 
$$\begin{aligned}
    \overline{K} \in \mathbb{R}^L := \kappa_L(\overline{A}, \overline{B}, \overline{C}) := (\overline{C}\overline{A}^i\overline{B})_{i \in [L]} = (\overline{C}\overline{B}, \overline{C}\overline{A}\overline{B}, ..., , \overline{C}\overline{A}^{L-1}\overline{B})
\end{aligned}$$

<br>
## Parallel Associative Scan

Consider an example for illustrating a parallel associative scan (or
parallel prefix sum/scan): $$\begin{aligned}
    x_t  &= \overline{A} x_{t-1} + \overline{B} u_t \\
\end{aligned}$$ 

We unroll the recursive equation with $$x_0 = 0$$.

$$\begin{aligned}
    x_1  &= \overline{B} u_1 \\
    x_2  &= \overline{A} x_1 + \overline{B} u_2 = \overline{A}\overline{B} u_1 + \overline{B} u_2\\
    x_3  &= \overline{A} x_2 + \overline{B} u_3 = \overline{A}(\overline{A}\overline{B} u_1 + \overline{B} u_2) + \overline{B} u_3 = \overline{A}^2\overline{B} u_1 + \overline{A}\overline{B} u_2 + \overline{B} u_3\\
    x_4  &= \overline{A} x_3 + \overline{B} u_4 = \overline{A}(\overline{A}^2\overline{B} u_1 + \overline{A}\overline{B} u_2 + \overline{B} u_3) + \overline{B} u_4 = \overline{A}^3\overline{B} u_1 + \overline{A}^2\overline{B} u_2 + \overline{A}\overline{B} u_3 + \overline{B} u_4\\
\end{aligned}$$

Define an operand $$e_i = (\overline{A}, \overline{B}u_i)$$ and operator
$$e_i \cdot e_j$$ for parallel associative scan such that

$$\begin{aligned}
e_1 \cdot e_2 &= (\overline{A}, \overline{B}u_1) \cdot (\overline{A}, \overline{B}u_2) = (\overline{A}^2, \overline{A}\overline{B}u_1 + \overline{B}u_2) = (\overline{A}^2, x_2) \\
e_1 \cdot e_2 \cdot e_3 &= (\overline{A}^2, \overline{A}\overline{B}u_1 + \overline{B}u_2) \cdot (\overline{A}, \overline{B}u_3) = (\overline{A}^3, \overline{A}^2\overline{B} u_1 + \overline{A}\overline{B} u_2 + \overline{B} u_3) = (\overline{A}^2, x_3) \\
\end{aligned}$$ 

Therefore, we can parallelize the computation with a
parallel associative scan into a tree structure, for example, $$x_4$$ can
be computed by 

$$\begin{aligned}
(e_1 \cdot e_2) \cdot (e_3 \cdot e_4) &=  (\overline{A}^2, \overline{A}\overline{B}u_1 + \overline{B}u_2) \cdot  (\overline{A}^2, \overline{A}\overline{B}u_3 + \overline{B}u_4) \\
& =  (\overline{A}^4, \overline{A}^3\overline{B} u_1 + \overline{A}^2\overline{B} u_2 + \overline{A}\overline{B} u_3 + \overline{B} u_4) \\
& =  (\overline{A}^4, x_4) \\
\end{aligned}$$

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
