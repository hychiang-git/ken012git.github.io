---
layout: post
title: "HALO: Hadamard-Assisted Lower-Precision Optimization for LLMs"
date: 2025-02-16
description: "HALO: Hadamard-Assisted Lower-Precision Optimization for LLMs"
tags: ml, paper, llm, quantization, training
comments: true
---
<style>
li {
    font-size: 1.1em; /* Adjust as needed */
}
</style>

# [HALO: Hadamard-Assisted Lower-Precision Optimization for LLMs](https://arxiv.org/abs/2501.02625)
> [TL;DR] 
> The paper proposes a quantized fine-tuning framework that exploits low bit-width matrix multiplication hardware units by performing quantization online for the inputs, weights, outputs, and their gradients with Hadamard transforms.

## Highlights
- Provide and experiment with different settings: [HALO-0, HALO-1, HALO-2. Speedup: HALO-0 > HALO-1 > HALO-2, Accuracy: HALO-2 > HALO-1 > HALO-0](#halo-levels)
- Support and experiment with different data types: FP8, INT8, and FP6 for forward and backward passes, exploiting acceleration from *low bit-width matrix multiplication* hardware units
- Support Fully Sharded Data Parallel (FSDP) scheme to enable further savings by performing *low-precision communication*
- Support full fine-tuning (FFT) and parameter-efficient fine-tuning (PEFT) methods such as LoRA
- Demonstrate accuracy improvements on downstream tasks for LLAMA-family models in quantized fine-tuning
- Demonstrate practical speedups in both FFT and FSDP cases
- Will open-source the kernel implementation at [https://github.com/IST-DASLab/HALO](https://github.com/IST-DASLab/HALO)

## Summary
- **Observation 1**: The quantization errors of the forward activations deviate the gradient direction (compare Figures (b) and (c)).
{% include figure.html path="assets/img/posts/halo/Halo_gradient_cosine.png" title="example image" class="img-fluid rounded z-depth-1" %}

<br>
- **Observation 2**: The outliers of the gradient with respect to the layer output $$\mathbf{E}_{\mathbf{Y}}$$ can only be eliminated by a [left-hand Hadamard transformation](#hadamard-transforms) (see the figure on the right).
{% include figure.html path="assets/img/posts/halo/Halo_lefthand_had.png" title="example image" class="img-fluid rounded z-depth-1" %}

<br>
- **The problem statement**: the large outliers in forward activations are difficult to represent in low bit-width data types.
- **The solution**: The paper addresses this issue by transforming the forward activations online into a smoother space using Hadamard matrices.
- **The quantized fine-tuning framework**: The proposed method quantizes the transformed inputs, weights, outputs, and their gradients with low bit-width data types (e.g., FP8, INT8, and FP6) in an online fashion to leverage acceleration from low bit-width matrix multiplication hardware units for forward and backward passes.
{% include figure.html path="assets/img/posts/halo/Halo_overview.png" title="example image" class="img-fluid rounded z-depth-1" %}

<br>
- **Different strategies**: The paper studies the placement of Hadamard rotations in both forward and backward passes. The left-hand Hadamard transformation (in the red rectangle) is applied to $$\mathbf{E}_{\mathbf{Y}}$$.
{% include figure.html path="assets/img/posts/halo/Halo_settings.png" title="example image" class="img-fluid rounded z-depth-1" %}

<br>

## Experiments

#### Compared with Baselines
- Left: Accuracy, Right: Relative speedup
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/posts/halo/Halo_vs_baselines_accuracy.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/posts/halo/Halo_vs_baselines_speedups.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

#### HALO Levels
- Accuracy
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/posts/halo/Halo_level_accuracy.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
- Relative speedup
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/posts/halo/Halo_level_speedups.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
## Notations

#### The forward and backward passes
- Let matrices $$\mathbf{X} \in \mathbb{R}^{b \times m}$$, $$\mathbf{W} \in \mathbb{R}^{n \times m}$$, and $$\mathbf{Y} \in \mathbb{R}^{b \times n}$$ be the inputs, weights, and outputs with batch size $$b$$.
- $$\mathbf{E}_{\mathbf{X}}$$, $$\mathbf{G}$$, and $$\mathbf{E}_{\mathbf{Y}}$$ are the gradients w.r.t. the inputs, weights, and outputs, respectively. The authors also refer $$\mathbf{E}_{\mathbf{X}}$$ and $$\mathbf{E}_{\mathbf{Y}}$$ as **errors**.
- The forward and backward calculations are defined as
$$
\begin{align}
    \mathbf{Y} &= \mathbf{X} \cdot \mathbf{W}^{\top} \quad &\textbf{(Forward)} \\
    \mathbf{G} &= \mathbf{E}_{\mathbf{Y}}^{\top} \cdot \mathbf{X} \quad &\textbf{(Gradient)} \\
    \mathbf{E}_{\mathbf{X}} &= \mathbf{E}_{\mathbf{Y}} \cdot \mathbf{W} \quad &\textbf{(Error)}
\end{align}
$$

#### Quantization
- Given half-precision (FP16) matrices $$\mathbf{A} \in \mathbb{R}^{m \times k}$$ and $$\mathbf{B} \in \mathbb{R}^{k \times n}$$
- $$\mathbf{A}_Q$$, $$\mathbf{B}_Q$$ are the quantized copies of $$\mathbf{A}$$ and $$\mathbf{B}$$.
- Low precision matrix multiplication $$\mathbf{Y} = \mathbf{A_Q B_Q}$$

#### Hadamard Transforms
- No Hadamard Case: $$\mathbf{Y} = \mathbf{A_Q B_Q}$$
- Left Case: $$\require{mathtools}\prescript{\mathbf{H}}{}{\mathbf{Y}} = \mathbf{H_m  (H_m^T A)_Q B_Q }$$
- Right Case: $$\mathbf{Y^H} = \mathbf{ A_Q (B H_n)_Q H_n^T}$$
- Middle Case: $$\stackrel{\mathbf{H}}{\mathbf{Y}} = \mathbf{(AH_k)_Q(H_k^TB)_Q}$$


