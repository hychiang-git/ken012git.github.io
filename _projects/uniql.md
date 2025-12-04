---
layout: page
title: UniQL
full_title: "UniQL: Unified Quantization and Low-rank Compression for Adaptive Edge LLMs"
authors: Hung-Yueh Chiang, Chi-Chih Chang, Yu-Chen Lu, Chien-Yu Lin, Kai-Chiang Wu, Mohamed S. Abdelfattah, Diana Marculescu
description: "Quantization and Low-rank Compression for Edge LLMs"
img: assets/img/publication_preview/uniql_blog.jpg
importance: 1
category: research
---

<style>  
li {  
    font-size: 1.1rem; /* Adjust as needed */  
}  
</style>  

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

<div style="text-align: center; padding-bottom: 1rem;">
<!-- <abbr class="badge" style="background-color:#00369f; margin-left:0.1rem; margin-right:0.1rem; font-size:1.1rem;">Arxiv</abbr> -->
<abbr class="badge" style="background-color:#BF5700; margin-left:0.1rem; margin-right:0.1rem; font-size:1.1rem; width:80px; display:inline-block; text-align:center;">Arxiv</abbr>
</div>

<div class="authors"> 
    <a href="https://hychiang.info">Hung-Yueh Chiang</a><sup>1</sup>,
    <a href="https://ccchang.info/">Chi-Chih Chang</a><sup>2</sup>, 
    <a href="https://scholar.google.com/citations?user=iTlHWjwAAAAJ&hl=zh-TW">Yu-Chen Lu</a><sup>3</sup>,
    <a href="https://cylinbao.github.io/">Chien-Yu Lin</a><sup>4</sup>,
    <br>
    <a href="https://people.cs.nycu.edu.tw/~kcw/">Kai-Chiang Wu</a><sup>3</sup>,
    <a href="https://www.mohsaied.com/">Mohamed S. Abdelfattah</a><sup>2</sup>,
    <a href="https://users.ece.utexas.edu/~dianam/">Diana Marculescu</a><sup>1</sup>
</div>
<div class="authors">
    <sup>1</sup>The University of Texas at Austin,
    <sup>2</sup>Cornell University, <br>
    <sup>3</sup>National Yang Ming Chiao Tung University,
    <sup>4</sup>University of Washington
</div>
<div style="text-align: center; margin-top:12px;">
    <a href="https://arxiv.org/abs/2512.03383"><i class="fa fa-file-pdf-o" style="font-size:24px;color"></i><b> Paper </b></a> &nbsp;
    <a href="https://github.com/enyac-group/UniQL"><i class="fa fa-github" style="font-size:24px;color"></i><b> Code </b></a> &nbsp;
    <a href="https://huggingface.co/ut-enyac"><span style="font-size: 22px;">&#129303;</span><b> Models </b></a>
</div>


<br>
<div style="text-align: center;">
<p style="font-family: Comic Neue; font-size: 1.4rem;">
    📚 Unified support Transformers, SSMs, and hybrid models <br>
    🔗 One-pass framework for quantization + structured low-rank pruning <br>
    ⚡ <strong>2.7×–3.4×</strong> latency speedups, <strong>4×–5.7×</strong> memory reductions <br>
</p>
</div>
<div class="row mt-3">
    <div class="col-sm-12 mt-3 mt-md-0 offset-0">
        {% include figure.html path="assets/img/projects/uniql/uniql.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# Supporting for Transformer and Mamba blocks
- Joint weight decomposition. (The group of weights is shown in the same background color)
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include gif.html path="assets/img/projects/uniql/modular.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# Joint design quantization and structured pruning
- Fused RoPE to support and accelerate pruned Q and K
- Quantization-aware SVD decomposition to reduce the quantization errors
<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/uniql/kernel_svd.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# One-pass framework supporting all pruning rates
- (a) <b>Pseudo-inverse-free</b>, <b>quantization-aware</b>, and <b>state-aware</b> matrix decomposition methods for the grouped weights to obtain sorted weights
- (b) During fine-tuning, we sample global pruning rates, and masked out the weight channels 
- (c) The refined patches are fused into the weights, followed by model quantization
for deployment
- (d) Based on the system utilization, we perform <b>on-device adaptive pruning</b> of the quantized model.
<div class="row mt-4">
    <div class="col-sm mt-3 mt-md-0 offset-0">
        {% include figure.html path="assets/img/projects/uniql/one-pass.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>



# Main results
<div class="row mt-4">
    <div class="col-sm-10 mt-4 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/uniql/main_results.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>




# Citation
{% raw %}
```latex
@article{chiang2025uniql,
  title={UniQL: Unified Quantization and Low-rank Compression for Adaptive Edge LLMs},
  author={Chiang, Hung-Yueh and Chang, Chi-Chih and Lu, Yu-Chen and Lin, Chien-Yu and Wu, Kai-Chiang and Abdelfattah, Mohamed S. and Marculescu, Diana},
  journal={arXiv preprint arXiv:2512.03383},
  year={2025},
}

```
{% endraw %}

<br>
# Acknowledgements
This work was supported in part by the ONR Minerva program, NSF CCF Grant No. 2107085, iMAGiNE - the Intelligent Machine Engineering Consortium at UT Austin, UT Cockrell School of Engineering Doctoral Fellowships, NSF CAREER Grant No. 2339084, Nvidia research gift, and Taiwan’s NSTC Grant No. 111-2221-E-A49-148-MY3.