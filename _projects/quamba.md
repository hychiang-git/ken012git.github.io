---
layout: page
title: Quamba
full_title: "Quamba: A Post-Training Quantization Recipe for Selective State Space Models"
authors: Hung-Yueh Chiang, Chi-Chih Chang, Natalia Frumkin, Kai-Chiang Wu, Diana Marculescu
description: "Quamba: A Post-Training Quantization Recipe for Mamba"
img: assets/img/publication_preview/quamba_blog.jpg
importance: 8
category: research
---

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

<div style="text-align: center; padding-bottom: 1rem;">
<abbr class="badge" style="background-color:#00369f; margin-left:0.1rem; margin-right:0.1rem; font-size:1.1rem;">ICLR 2025</abbr>
</div>

<div class="authors">
    <a href="https://hychiang.info">Hung-Yueh Chiang</a><sup><sub>*</sub>1</sup>,
    <a href="https://ccchang.info/">Chi-Chih Chang</a><sup><sub>*</sub>2</sup>,
    <a href="https://www.nfrumkin.com/">Natalia Frumkin</a><sup>1</sup>,
    <a href="https://people.cs.nycu.edu.tw/~kcw/">Kai-Chiang Wu</a><sup>2</sup>,
    <a href="https://users.ece.utexas.edu/~dianam/">Diana Marculescu</a><sup>1</sup>
</div>
<div class="authors">
    <sup>1</sup>The University of Texas at Austin,
    <sup>2</sup>National Yang Ming Chiao Tung University
</div>
<div style="text-align: center; font-family: Times;"> <sup>*</sup> Equal contribution</div>

<div style="text-align: center; margin-top:12px;">
    <a href="https://arxiv.org/abs/2410.13229"><i class="fa fa-file-pdf-o" style="font-size:24px;color"></i><b> Paper</b></a>&nbsp;
    <a href="https://github.com/enyac-group/Quamba"><i class="fa fa-github" style="font-size:24px;color"></i><b> Code</b></a>&nbsp;
    <a href="https://huggingface.co/ut-enyac"><span style="font-size: 22px;">&#129303;</span><b> Models</b></a>
</div>


<br>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/projects/quamba/quamba_main.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

<div style="text-align: center;">
    <p style="font-family: Comic Neue;">
    :zap: <b>8-bit quantization (W8A8) for Mamba blocks </b> &nbsp; &nbsp;
    :rocket: <b>1.7 <span>&#215;</span> speedup on Orin Nano 8G </b> &nbsp; &nbsp;
    :small_red_triangle_down: <b>2<span>&#215;</span> memory reduction</b>
    </p>
</div>

<br>

# Real-time Generation on Edge GPUs
We compared Quamba 2.8B with Mamba 2.8B on a NVIDIA Orin Nano 8G. Quamba (W8A8) is $$1.7\times$$ faster than Mamba (FP16) on the Nano. The real-time generation speed is shown in the demo.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include gif.html path="assets/img/projects/quamba/quamba_demo.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
# Long Input Sequences on Edge GPUs
We compared Quamba with an 8-bit transformer on a NVIDIA Orin Nano 8G. Quamba is capable of handling long input sequences (over 8k tokens) with limited memory and computational resources on edge devices.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include gif.html path="assets/img/projects/quamba/quamba_opt.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


<br>
# Zero-shot Accuracy
Zero-shot accuracy of quantized models on six common sense tasks. Quamba is a *static per-tensor* quantization method that closes the performance gap and outperforms the same-sized
Transformers (Pythia) in accuracy. (**Bold** is the best, and <ins>underline</ins> is the second best.)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/projects/quamba/quamba_zeroshot.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
# Perplexity Evaluation
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
    Perplexity results of different quantization methods applied on Mamba families. We evaluate the quantized models on a subset of Pile and Wikitext2 datasets. SmQ stands for SmoothQuant. Quamba is a <i>static per-tensor</i> quantization method that closes the performance gap in terms of perplexity and outperforms the same-sized Transformers (Pythia).
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/projects/quamba/quamba_ppl.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


<br>
# Quantizing Jamba: A Large-Scale Hybrid Mamba-Transformer LLM
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
    Jamba is a hybrid transformer-mamba language model with <b>52B parameters</b>, built with Self-attention, Mixture of Experts (MoE), and Mamba blocks. We experiment and combine off-the-shelf quantization methods with our method. The zero-shot LAMBADA accuracy is reported.
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/projects/quamba/quamba_jamba.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# Presentation
<iframe width="560" height="315" src="https://www.youtube.com/embed/-AdqhRhN4xc?si=Zd2KSNFHy2ZBA5VI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<br>

# Citation
{% raw %}
```latex
@inproceedings{chiang2025quamba,
  title = {Quamba: A Post-Training Quantization Recipe for Selective State Space Models},
  author = {Chiang*, Hung-Yueh and Chang*, Chi-Chih and Frumkin, Natalia and Wu, Kai-Chiang and Marculescu, Diana},
  booktitle = {International Conference on Learning Representations (ICLR)},
  year = {2025},
}
```
{% endraw %}

<br>
# Acknowledgements
This work was supported in part by the ONR Minerva program, NSF CCF Grant No. 2107085, iMAGiNE - the Intelligent Machine Engineering Consortium at UT Austin, UT Cockrell School of Engineering Doctoral Fellowships, and Taiwan’s NSTC Grant No. 111-2221-E-A49-148-MY3.