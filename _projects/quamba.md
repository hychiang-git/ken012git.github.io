---
layout: page
title: Quamba
full_title: "Quamba: Post-training Quantization for Selective State Space Models"
authors: Hung-Yueh Chiang, Chi-Chih Chang, Natalia Frumkin, Kai-Chiang Wu, Diana Marculescu
description: Post-training Quantization for Selective State Space Models
img: assets/img/publication_preview/quamba.jpg
importance: 1
category: research
---

<div style="text-align: center;">
    <p>(Under reviewing, more details are coming soon...)</p>
</div>


<div class="authors"> <a href="https://hychiang.info">Hung-Yueh Chiang</a><sup><sub>*</sub>1</sup>, <a href="https://github.com/shadowpa0327">Chi-Chih Chang</a><sup><sub>*</sub>2</sup>, <a href="https://www.nfrumkin.com/">Natalia Frumkin</a><sup>1</sup>, <a href="https://people.cs.nycu.edu.tw/~kcw/">Kai-Chiang Wu</a><sup>2</sup>, <a href="https://users.ece.utexas.edu/~dianam/">Diana Marculescu</a><sup>1</sup></div>
<div class="authors"> <sup>1</sup> The University of Texas at Austin, <sup>2</sup>National Yang Ming Chiao Tung University</div>
<div style="text-align: center; font-family: Times;"> <sup>*</sup> Equal contribution</div>

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

