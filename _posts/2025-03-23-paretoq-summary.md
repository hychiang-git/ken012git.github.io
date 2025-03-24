---
layout: post
title: "ParetoQ: Scaling Laws in Extremely Low-bit LLM Quantization"
date: 2025-03-23
description: "ParetoQ: Scaling Laws in Extremely Low-bit LLM Quantization"
tags: ml paper llm quantization
comments: true
---

<style>  
li {  
    font-size: 1.1rem; /* Adjust as needed */  
}  
</style>  

# [ParetoQ: Scaling Laws in Extremely Low-bit LLM Quantization](https://arxiv.org/abs/2502.02631)  
> [TL;DR]  
> The paper introduces ParetoQ, a unified framework that compares LLM quantization across 1-bit, 1.58-bit, 2-bit, 3-bit, and 4-bit settings. It discovers a key transition between 2-bit and 3-bit quantization, where models retain their original representations at 3-bit and higher, but undergo substantial changes at lower bit widths. ParetoQ shows that 2-bit quantization is a strong alternative to 4-bit due to its superior efficiency-accuracy trade-offs.



## Highlights  
- Demonstrates that 2-bit, 3-bit, and ternary quantization often outperform 4-bit in terms of accuracy-memory trade-offs.
<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 offset-3">
        {% include figure.html path="assets/img/posts/paretoq/pareto.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>
- Identifies a sharp transition between 2-bit and 3-bit quantization, where 3-bit models and above retain pre-trained distributions, while 2-bit models undergo major representation shifts.
<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 offset-3">
        {% include figure.html path="assets/img/posts/paretoq/compansate_reconstruct.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>
- Quantization-aware training (QAT) consistently surpasses both post-training quantization (PTQ, no fine-tuning) and QAT from scratch.
<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 offset-3">
        {% include figure.html path="assets/img/posts/paretoq/qat_vs_ptq.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>
- Propose a refined quantization functions, Stretched Elastic Quant (SEQ), for low-bit settings.
$$
\mathbf{W}_Q^i = \alpha \left( \left\lfloor \text{Clip} \left( \frac{\mathbf{W}_R^i}{\alpha}, -1, 1 \right) \times \frac{k}{2} - 0.5 \right\rfloor + 0.5 \right) / k \times 2
$$
$$
\mathbf{W}_Q^i = \alpha \mathbf{\hat{W}}_Q^i
= 
\begin{cases} 
\alpha \cdot \text{Sign}(\mathbf{W}_R^i), & \text{if } N_{bit} = 1 \\ 
\alpha \left( \left\lfloor \text{Clip} \left( \frac{\mathbf{W}_R^i}{\alpha}, -1, 1 \right) \times \frac{k}{2} - 0.5 \right\rfloor + 0.5 \right) / k \times 2, & \text{if } N_{bit} = 1.58, 2 \\ 
\alpha \lfloor \text{Clip} \left( \frac{\mathbf{W}_R^i}{\alpha}, n, p \right) \rfloor, & \text{if } N_{bit} = 3, 4 
\end{cases}
$$
<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/posts/paretoq/seq.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


## Summary 
- **Observation 1**: Recent studies on scaling laws in the low-precision domain have reached conflicting conclusions.
  - [Dettmers & Zettlemoyer](https://proceedings.mlr.press/v202/dettmers23a) and [Kumar et al](https://arxiv.org/abs/2411.04330) argue that 4-bit or 6-bit quantization often resides on the Pareto frontier, balancing accuracy and efficiency.
  - In contrast, [Ma et al.](https://storage.prod.researchhub.com/uploads/papers/2024/02/29/2402.17764.pdf) and [Kaushal et al.](https://arxiv.org/abs/2407.12327) suggest that bit-widths as low as 1.58 bits per parameter offer significant potential for optimal scaling trade-offs.
- **Observation 2**: Prior studies overlook the impact of the training scheme, denoted as $$\mathbf{S}_{\text{train}}$$, and the bit-specific quantization function $$\mathcal{F}$$.
- **The problem statement**: How to determine the optimal trade-off between bit-width and model size while ensuring accuracy?
- **The solution**: The authors propose a scaling law $$\mathcal{L}(\mathcal{N}, \mathcal{D}, \mathcal{P}, \mathbf{S}_{\text{train}}, \mathcal{F})$$ comprising five dimensions, and systematically optimizes quantization functions and training schemes across different bit-widths.
  - Introduces Stretched Elastic Quantization (SEQ), which balances quantization grids for 2-bit and ternary settings.
  - Applies learnable quantization ranges, outperforming static min-max methods.
- **The proposed framework**: The quantized framework evaluates models under 1-bit, 1.58-bit, 2-bit, 3-bit, and 4-bit precision.


## Experiments  

- Accuracy-compression and Accuracy-speed Trade-off
<div class="row mt-3">
    <div class="col-sm-12 mt-3 mt-md-0 offset-0">
        {% include figure.html path="assets/img/posts/paretoq/pareto_models_latency.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


- 2-bit / 3-bit / 4-bit Comparisons
<div class="row mt-3">
    <div class="col-sm-12 mt-3 mt-md-0 offset-0">
        {% include figure.html path="assets/img/posts/paretoq/bits_2_3_4.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

- 1.58-bit Comparison on Sub-8B Models
  - Note: floating-point LLaMA-3 3B model achieves 69.9 accuracy
<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 offset-3">
        {% include figure.html path="assets/img/posts/paretoq/sub_8b_model.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

- Main Results
<div class="row mt-3">
    <div class="col-sm-8 mt-3 mt-md-0 offset-2">
        {% include figure.html path="assets/img/posts/paretoq/main_table.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

## Conclusions
- 2-bit quantization outperforms 4-bit in efficiency-accuracy trade-offs.
- Fine-tuning is crucial for sub-4-bit quantization, especially for binary and ternary models.
- Quantization-aware training (QAT) finetuning consistently surpasses both post-training quantization (PTQ, no fine-tuning) and QAT from scratch 
- QAT serves as a compensation mechanism for bit widths above 2-bit and as a reconstruction process for bit widths below 2-bit, where weights adapt to form new representations.
- Extreme low-bit quantization is highly sensitive to quantization function selection, with no single optimal function for all bit widths.