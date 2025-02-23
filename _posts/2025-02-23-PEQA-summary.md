---
layout: post
title: "Memory-Efficient Fine-Tuning of Compressed Large Language Models via sub-4-bit Integer Quantization"
date: 2025-02-23
description: "Memory-Efficient Fine-Tuning of Compressed Large Language Models via sub-4-bit Integer Quantization"
tags: ml paper llm quantization training
comments: true
---

<style>  
li {  
    font-size: 1.1em; /* Adjust as needed */  
}  
</style>  

# [Memory-Efficient Fine-Tuning of Compressed Large Language Models via sub-4-bit Integer Quantization](https://arxiv.org/abs/2305.14152)  
> [TL;DR]  
> PEQA is a novel fine-tuning approach that integrates Parameter-Efficient Fine-Tuning (PEFT) with *weight-only* quantized LLMs by updating only the quantization scales, preserving low-bit integer weight matrices. This results in huge memory savings, seamless task adaptation, and inference acceleration.

## Highlights  
- Integrates PEFT with quantized LLMs, updating only the **quantization scales** while keeping integer matrices frozen.  
- **Reduces memory** consumption during **fine-tuning** and **deployment**, making LLM adaptation feasible even for resource-constrained settings.  
- Maintains quantization benefits post fine-tuning, ensuring **accelerated inference**.  
- Demonstrates resilience in performance recovery, even for **sub-4-bit quantized models**, on large-scale instruction datasets.  
- **Scales up to 65B parameter models** while achieving performance close to full-precision fine-tuning.  
{% include figure.html path="assets/img/posts/peqa/results_overview.png" title="PEQA Overview" class="img-fluid rounded z-depth-1" %} 
<br>

## Summary  
- **Problem Statement**: LLM fine-tuning is memory-intensive, even with PEFT, as full-precision weights remain a bottleneck. Quantization can reduce memory but is typically applied post-training, limiting adaptability.  
- **Solution**: PEQA bridges this gap by fine-tuning only the *quantization scales* of a pre-quantized LLM while keeping the integer weights frozen. This enables task-specific adaptation with minimal overhead.
- **PEQA Framework**:  
  - **Step 1 Decomposition**: Pre-trained model weights are quantized into sub-4-bit integers with associated scaling factors.
  - **Step 2 Fine-tuning**: Only the quantization scales are updated while maintaining the frozen integer matrix, significantly reducing learnable parameters.  

{% include figure.html path="assets/img/posts/peqa/framework_overview.png" title="PEQA Overview" class="img-fluid rounded z-depth-1" %}  
<br>


## Key Advantages  

#### Memory Efficiency  
- **Fine-tunes only the quantization scales**, significantly reducing memory overhead.  
- **Optimized for low-bit integer quantization (≤ 4-bit)** while maintaining high accuracy.  

#### Seamless Task Switching  
- PEQA enables **quick and efficient adaptation** across different tasks by swapping quantization scales instead of retraining entire models.  

#### Faster Inference  
- The **frozen integer matrix remains intact**, ensuring post-fine-tuning speedup using quantized inference kernels.  
<br>

## Experiments  

#### Memory and General Comparison  
{% include figure.html path="assets/img/posts/peqa/framework_comparison.png" title="Instruction-Tuning Results" class="img-fluid rounded z-depth-1" %}  
<br>

#### PEQA vs. QAT vs. PEFT+PTQ  
- PEQA achieves performance close to QAT, significantly outperforming LoRA + PTQ at **3-bit and 4-bit** precision.  
- Lower perplexity indicates effective fine-tuning of quantized models without sacrificing accuracy.  
{% include figure.html path="assets/img/posts/peqa/peqa_vs_baselines_ppl.png" title="Instruction-Tuning Results" class="img-fluid rounded z-depth-1" %}  
<br>


#### Instruction-Tuning with Alpaca Dataset  
- Evaluated on **common-sense reasoning and in-context learning tasks** (ARC, PIQA, HellaSwag).  
- **Performance comparable to LoRA**, with additional memory savings and inference acceleration.  

{% include figure.html path="assets/img/posts/peqa/peqa_instruction_tuning.png" title="Instruction-Tuning Results" class="img-fluid rounded z-depth-1" %}  
<br>

## Notations  

#### Quantized Weights and Fine-Tuning  
- Weight-only asymmetric quantization:  
Given a fully-connected layer $$\mathbf{W}_0 \in \mathbb{R}^{n \times m}$$, a given bit-width $$b$$, per-channel scales and zero-points $$\mathbf{s}_0, \mathbf{z}_0 \in \mathbb{R}^{n \times 1}$$, asymmetric quantized pre-trained weights $$\widehat{\mathbf{W}}_0$$ can be written as  

$$
\widehat{\mathbf{W}}_0 = \mathbf{s}_0 \cdot \overline{\mathbf{W}}_0 = \mathbf{s}_0 \cdot \left( \text{clamp} \left( \left\lfloor \frac{\mathbf{W}_0}{\mathbf{s}_0} \right\rfloor + \mathbf{z}_0, 0, 2^b - 1 \right) - \mathbf{z}_0 \right),
$$

- PEQA fine-tuning modifies only the quantization scale by:  
$$
\widehat{\mathbf{W}} = (\mathbf{s}_0 + \Delta s) \cdot \overline{\mathbf{W}}_0 = (\mathbf{s}_0 + \Delta s) \cdot \left( \text{clamp} \left( \left\lfloor \frac{\mathbf{W}_0}{\mathbf{s}_0} \right\rfloor + \mathbf{z}_0, 0, 2^b - 1 \right) - \mathbf{z}_0 \right)
$$
where $$\overline{\mathbf{W}}_0$$ is frozen, and $$\Delta \mathbf{s} \in \mathbb{R}^{n \times 1}$$ represents the gradient update of $$\mathbf{s}_0$$ obtained by adaptation to a downstream task. 
<br>

## Conclusion  
PEQA presents a **memory-efficient fine-tuning** approach for quantized LLMs. By updating only the quantization scales while keeping integer matrices fixed, PEQA achieves:  
- **Comparable accuracy to full-precision fine-tuning**  
- **Significant memory savings (up to 4× reduction)**  
- **Seamless adaptation to new tasks**  
- **Faster inference without additional post-processing**  

PEQA enables **scalable and efficient model adaptation** for large-scale language models, ensuring practical deployment on memory-constrained devices.  
