---
layout: post
title: State Space Models
date: 2024-07-11
description: State Space Models
tags: ml paper algorithm SSM
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
  <li><a href="#discretize-continuous-state-space-model">Discretize Continuous State Space Model</a></li>
  <ul>
    <li><a href="#bilinear-transform">Bilinear Transform</a></li>
    <li><a href="#zero-order-hold-transform">Zero-order Hold Transform</a></li>
  </ul>
  <li><a href="#discrete-time-state-space-model">Discrete-time State Space Model</a></li>
  <ul>
    <li><a href="#convolution-form<">Convolution Form</a></li>
    <li><a href="#parallel-associative-scan">Parallel Associative Scan</a></li>
  </ul>
  <li><a href="#references">References</a></li>
</ul>
</details>

<br>

# Introduction
This document provides the mathematic derivations of the State Space Models that help readers understand the formulas. The contents are extracted and summarized from the original papers cited in <a href="#references">references</a>.

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
    \frac{d}{dt}h(t) = A h(t) + B x(t).
\end{aligned}
$$

From the differential equation, we have $$\begin{aligned}
    h(t+\Delta t) - h(t) = \int_{s=t}^{s=t+\Delta t} A h(s) + B x(s)ds .
\end{aligned}$$

## Bilinear Transform

By the Trapezoidal rule, we have 

$$
\begin{aligned}
    h(t+\Delta t) - h(t) &= \Delta t \frac{(Ah(t) + Bx(t) + Ah(t+\Delta t) + B x(t+\Delta t))}{2} \\
    &= \frac{\Delta t}{2} Ah(t) + \frac{\Delta t}{2}Bx(t) + \frac{\Delta t}{2}Ah(t+\Delta t) + \frac{\Delta t}{2}Bx(t+\Delta t)
\end{aligned}
$$

By combining the terms, we have 

$$
\begin{aligned}
    \textcolor{red}{h(t+\Delta t)} - \frac{\Delta t}{2}A\textcolor{red}{h(t+\Delta t)} &= \textcolor{green}{h(t)} + \frac{\Delta t}{2} A \textcolor{green}{h(t)} + \textcolor{blue}{\frac{\Delta t}{2}B} x(t) + \textcolor{blue}{\frac{\Delta t}{2}B} x(t+\Delta t) \\
    ( \mathbf{I} - \frac{\Delta t}{2}A)h(t+\Delta t) &= ( \mathbf{I}+ \frac{\Delta t}{2} A)h(t)  + \frac{\Delta t}{2} B(x(t) + x(t+\Delta t)) \\
    h(t+\Delta t) &= ( \mathbf{I} - \frac{\Delta t}{2}A)^{-1} ( \mathbf{I}+ \frac{\Delta t}{2} A) h(t) + \frac{\Delta t}{2} ( \mathbf{I} - \frac{\Delta t}{2}A)^{-1} B (x(t) +  x(t+\Delta t))
\end{aligned}
$$

Since $$x$$ is the constant and does no involve in the
integral, we have $$x(t) = x(t+\Delta t)$$ 

$$\begin{aligned}
    h(t+\Delta t) &= ( \mathbf{I} - \frac{\Delta t}{2}A)^{-1} ( \mathbf{I}+ \frac{\Delta t}{2} A) h(t) + \Delta t ( \mathbf{I} - \frac{\Delta t}{2} A)^{-1} B x(t+\Delta t)
\end{aligned}
$$

We can replace $$\frac{1}{2}$$ with a variable $$\alpha$$, so that we have
Generalized Bilinear Transformation (GBT). 

$$\begin{aligned}
    h(t+\Delta t) &= \textcolor{red}{( \mathbf{I} - \Delta t \alpha A)^{-1} ( \mathbf{I}+ \Delta t \alpha A)} h(t) + \textcolor{green}{\Delta t ( \mathbf{I} - \Delta t \alpha A)^{-1} B} x(t+\Delta t)
\end{aligned}$$

We can write the discretized state space model as 

$$\begin{aligned}
    h[t+1]  &= \overline{A} h[t] + \overline{B} x[t+1] \\
    y[t+1]    &= Ch[t+1]+Dx[t+1]
\end{aligned}$$

where $$\overline{A}, \overline{B}$$  are discretized matrices 

$$\begin{aligned}
    \overline{A} &= \textcolor{red}{(\mathbf{I} - \Delta t \alpha A)^{-1} ( \mathbf{I}+ \Delta t \alpha A)} \\
    \overline{B} &= \textcolor{green}{\Delta t ( \mathbf{I} - \Delta t \alpha A)^{-1} B}
\end{aligned}$$

<br>
## Zero-order Hold Transform (ZOH)

#### State solution

We derive the state solution from the continuous time-invariant dynamic equation  $$\frac{d}{dt}h(t) = A h(t) + Bx(t)$$ 
and move $$A h(t)$$ to the left-hand side

$$\begin{aligned}
    \textcolor{red}{ \frac{d}{dt}h(t) - A h(t) } =  Bx(t) .
\end{aligned}$$ 

Using the fact 

$$\begin{aligned}
    \textcolor{red}{\frac{d}{dt}[e^{-At}h(t)]} = e^{-At} \frac{d}{dt}h(t) - e^{-At} A h(t) = \textcolor{green}{e^{-At}} [ \textcolor{red}{\frac{d}{dt}h(t) - A h(t)} ]
\end{aligned}$$ 

, we have 

$$\begin{aligned}
    \textcolor{red}{\frac{d}{dt}[e^{-At}h(t)]} = \textcolor{green}{e^{-At}} Bx(t) \\
\end{aligned}$$ 

We integrate the both side from $$0$$ to $$t$$

$$\begin{aligned}
    \int_0^t \frac{d}{d \tau}[e^{-A\tau}h(\tau)] d\tau = \textcolor{red}{e^{-At}h(t) - h(0) = \int_0^t e^{-A\tau} Bx(\tau) d \tau} \\
\end{aligned}$$ 

and move $$h(0)$$ to the right-hand side

$$\begin{aligned}
    e^{-At}h(t) = h(0) + \int_0^t e^{-A\tau} Bx(\tau) d \tau
\end{aligned}$$ 

After multiplying $$e^{At}$$ on both sides, we have the
state solution 

$$
\label{state_solution}
\tag{eq:1}
\begin{aligned}
    h(t) = e^{At}h(0) + \int_0^t e^{A(t-\tau)} Bx(\tau) d \tau
\end{aligned}
$$ 

Note that $$\int_0^t e^{A(t-\tau)} Bx(\tau) d \tau$$ is a
convolution, which can be done efficiently by multiplying the scalars in
the frequency domain. We will use this result to derive the Zero-order
Hold discretization.

<br>
#### Zero-order Hold Discretization

We first define the relationship between discretized signals and
continuous signals. 

$$\begin{aligned}
    h[k] := h(k\Delta t), \quad \text{and} \quad x[k] := x(k\Delta t)
\end{aligned}$$

where $$h[x]$$ and $$c[k]$$ is a zero-order transformed
signals from $$h(x)$$ and $$x(x)$$ with a sampling step $$k$$.

Then using [[eq:1]](#eq:1), we have 

$$\begin{aligned}
    h[k+1] &:= h(\textcolor{red}{(k+1)\Delta t}) \\
    &= e^{A\textcolor{red}{(k+1)\Delta t}}h(0) + \int_0^{\textcolor{red}{(k+1)\Delta t}} e^{A(\textcolor{red}{(k+1)\Delta t}-\tau)} Bx(\tau) d \tau \\
    &= e^{A(k+1)\Delta t}h(0) + \int_{\textcolor{green}{0}}^{\textcolor{green}{k\Delta t}} e^{A((k+1)\Delta t-\tau)} Bx(\tau) d \tau + \int_{\textcolor{green}{k\Delta t}}^{\textcolor{green}{(k+1)\Delta t}} e^{A((k+1)\Delta t-\tau)} Bx(\tau) d \tau \\
    &= \textcolor{red}{e^{A\Delta t}}e^{Ak\Delta t}h(0) + \int_0^{k\Delta t} \textcolor{red}{e^{A\Delta t}} e^{A(k\Delta t-\tau)} Bx(\tau) d \tau + \int_{k\Delta t}^{(k+1)\Delta t} e^{A((k+1)\Delta t-\tau)} Bx(\tau) d \tau \\
    &= e^{A\Delta t}\left( \textcolor{green}{ e^{Ak\Delta t}h(0) + \int_0^{k\Delta t} e^{A(k\Delta t-\tau)} Bx(\tau) d \tau} \right) + \int_{k\Delta t}^{(k+1)\Delta t} e^{A((k+1)\Delta t-\tau)} Bx(\tau) d \tau \\
    &= e^{A\Delta t} \textcolor{green}{h(k\Delta t)} + \int_{k\Delta t}^{(k+1)\Delta t} e^{A((k+1)\Delta t-\tau)} Bx(\tau) d \tau \\
    &= e^{A\Delta t} h[k] + e^{A((k+1)\Delta t} B \textcolor{red}{\int_{k\Delta t}^{(k+1)\Delta t} e^{-A\tau} x(\tau) d \tau} \\
    &= e^{A\Delta t} h[k] + e^{A((k+1)\Delta t} B (\textcolor{red}{-A^{-1}e^{-A\tau}\Big|_{k\Delta t}^{(k+1)\Delta t}})x((k+1)\Delta t) \\
    &= e^{A\Delta t} h[k] + e^{A((k+1)\Delta t} B (\textcolor{red}{-A^{-1}e^{-A(k+1)\Delta t} + -A^{-1}e^{-A k\Delta t}}) x((k+1)\Delta t) \\
    &= e^{A\Delta t} h[k] + e^{A((k+1)\Delta t} B (\textcolor{red}{-A^{-1}})(e^{-A(k+1)\Delta t} + e^{-A k\Delta t}) x((k+1)\Delta t) \\
    &= e^{A\Delta t} h[k] + \textcolor{green}{e^{A((k+1)\Delta t}} B (-A^{-1}) (\textcolor{green}{e^{-A(k+1)\Delta t} + e^{-A k\Delta t}}) x((k+1)\Delta t) \\
    &= e^{A\Delta t} h[k] + BA^{-1} (\textcolor{green}{-\mathbf{I} + e^{A \Delta t}}) x[k+1] \\
    &= \textcolor{red}{e^{A\Delta t}} h[k] + \textcolor{green}{\frac{\Delta t B}{\Delta t A} (e^{A \Delta t} - \mathbf{I})} x[k+1] \\
\end{aligned}$$

Note that $$x$$ is constant from $$k\Delta t$$ to $$(k+1)\Delta t$$, and thereby $$x(k\Delta t) = x((k+1)\Delta t)$$

We can write the discretized state space model as 

$$\begin{aligned}
    h[t+1]  &= \overline{A} h[t] + \overline{B} x[t+1] \\
    y[t+1]    &= Ch[t+1]+Dx[t+1]
\end{aligned}$$

where $$\overline{A}, \overline{B}$$  are discretized matrices 

$$\begin{aligned}
    \overline{A} &= \textcolor{red}{e^{A\Delta t}} \\
    \overline{B} &= \textcolor{green}{ {(A \Delta t)}^{-1} (e^{A \Delta t} - \mathbf{I}) \Delta t B}
\end{aligned}$$

We can perform an Euler approximation to simplify $$B$$.
The first-order Taylor expansion of $$e^{A \Delta t}$$ around zero is given by:

$$\begin{aligned}
e^{A \Delta t} \approx I + A \Delta t
\end{aligned}$$

Therefore, we have

$$\begin{aligned}
    \overline{B} &= {(A \Delta t)}^{-1} (\textcolor{red}{e^{A \Delta t}} - \mathbf{I}) \Delta t B
    \approx {(A \Delta t)}^{-1} (\textcolor{red}{I + A \Delta t} - \mathbf{I}) \Delta t B =  \Delta t B
\end{aligned}$$


# Discrete-time State Space Model

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
\overline{B} &= {(\Delta t A)}^{-1} (e^{A \Delta t} - \mathbf{I}) \Delta t B \approx \Delta t B\\
\end{aligned}$$

<br>
## Convolution Form

Letting $$h_{-1} = 0$$ and unrolling the $$y_k$$ 

$$\begin{aligned}[c]
&\text{t=0}\\
h_0 &= \overline{B}x_0\\
y_0 &= C\overline{B}x_0 + Dx_0\\
\end{aligned}
\quad
\begin{aligned}[c]
&\text{t=1}\\
h_1 &= \overline{A}\overline{B}x_0 + \overline{B}x_1\\
y_1 &= C\overline{A}\overline{B}x_0 + C\overline{B}x_1 + Dx_1\\
\end{aligned}
\quad
\begin{aligned}[c]
&\text{t=2}\\
h_2 &= \overline{A}^2\overline{B}x_0 + \overline{A}\overline{B}x_1 + \overline{B}x_2\\
y_2 &= C\overline{A}^2\overline{B}x_0 + C\overline{A}\overline{B}x_1 + C\overline{B}x_2 + Dx_2\\
\end{aligned}$$ 

Therefore, when $$t=k$$ 

$$\begin{aligned}
h_k &= \overline{A}^k\overline{B}x_0 +...+ \overline{A}\overline{B}x_{k-1} + \overline{B}x_k\\
y_k &= C\overline{A}^k\overline{B}x_0 + C\overline{A}^{k-1}\overline{B}x_1 +...+ C\overline{A}\overline{B}x_{k-1} + C\overline{B}x_k + Dx_k \\
\end{aligned}$$

In other words, $$y_t$$ is a single (non-circular) convolution

$$\begin{aligned}
y_t = \sum_{\tau=0}^k \overline{C}\overline{A}^{\tau}\overline{B} x_{t-\tau}  + Dx_t
\end{aligned}$$

which can be computed very efficiently with FFTs,
provided that $$\overline{K}$$ is known. 

$$\begin{aligned}
    y_t = \overline{K} \ast x + Dx_t
\end{aligned}$$
where 
$$\begin{aligned}
    \overline{K} \in \mathbb{R}^L := \kappa_L(\overline{A}, \overline{B}, \overline{C}) := (\overline{C}\overline{A}^i\overline{B})_{i \in [L]} = (\overline{C}\overline{B}, \overline{C}\overline{A}\overline{B}, ..., , \overline{C}\overline{A}^{L-1}\overline{B})
\end{aligned}$$

<br>
## Parallel Associative Scan

Consider an example for illustrating a parallel associative scan (or
parallel prefix sum/scan): $$\begin{aligned}
    h_t  &= \overline{A} h_{t-1} + \overline{B} x_t \\
\end{aligned}$$ 

We unroll the recursive equation with $$h_0 = 0$$.

$$\begin{aligned}
    h_1  &= \overline{B} x_1 \\
    h_2  &= \overline{A} h_1 + \overline{B} x_2 = \overline{A}\overline{B} x_1 + \overline{B} x_2\\
    h_3  &= \overline{A} h_2 + \overline{B} x_3 = \overline{A}(\overline{A}\overline{B} x_1 + \overline{B} x_2) + \overline{B} x_3 = \overline{A}^2\overline{B} x_1 + \overline{A}\overline{B} x_2 + \overline{B} x_3\\
    h_4  &= \overline{A} h_3 + \overline{B} x_4 = \overline{A}(\overline{A}^2\overline{B} x_1 + \overline{A}\overline{B} x_2 + \overline{B} x_3) + \overline{B} x_4 = \overline{A}^3\overline{B} x_1 + \overline{A}^2\overline{B} x_2 + \overline{A}\overline{B} x_3 + \overline{B} x_4\\
\end{aligned}$$

Define an operand $$e_i = (\overline{A}, \overline{B}x_i)$$ and operator
$$e_i \cdot e_j$$ for parallel associative scan such that

$$\begin{aligned}
e_1 \cdot e_2 &= (\overline{A}, \overline{B}x_1) \cdot (\overline{A}, \overline{B}x_2) = (\overline{A}^2, \overline{A}\overline{B}x_1 + \overline{B}x_2) = (\overline{A}^2, h_2) \\
e_1 \cdot e_2 \cdot e_3 &= (\overline{A}^2, \overline{A}\overline{B}x_1 + \overline{B}x_2) \cdot (\overline{A}, \overline{B}x_3) = (\overline{A}^3, \overline{A}^2\overline{B} x_1 + \overline{A}\overline{B} x_2 + \overline{B} x_3) = (\overline{A}^2, h_3) \\
\end{aligned}$$ 

Therefore, we can parallelize the computation with a
parallel associative scan into a tree structure, for example, $$h_4$$ can
be computed by 

$$\begin{aligned}
(e_1 \cdot e_2 \cdot e_3 \cdot e_4) &= (e_1 \cdot e_2) \cdot (e_3 \cdot e_4) \\
&=  (\overline{A}^2, \overline{A}\overline{B}x_1 + \overline{B}x_2) \cdot  (\overline{A}^2, \overline{A}\overline{B}x_3 + \overline{B}x_4) \\
& =  (\overline{A}^4, \overline{A}^3\overline{B} x_1 + \overline{A}^2\overline{B} x_2 + \overline{A}\overline{B} x_3 + \overline{B} x_4) \\
& =  (\overline{A}^4, h_4) \\
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

<a id="4">[4]</a> 
<a href="https://en.wikipedia.org/wiki/State-space_representation">https://en.wikipedia.org/wiki/State-space_representation</a> 