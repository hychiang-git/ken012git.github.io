---
layout: page
title: Quamba2
full_title: "Quamba2: A Robust and Scalable Post-training Quantization Framework for Selective State Space Models"
authors: Hung-Yueh Chiang, Chi-Chih Chang, Natalia Frumkin, Kai-Chiang Wu, Mohamed S. Abdelfattah, Diana Marculescu
description: "A Quantization Framework for Selective State Space Models"
img: assets/img/publication_preview/quamba2_blog.jpg
importance: 7
category: research
---

<style>  
li {  
    font-size: 1.1rem; /* Adjust as needed */  
}  
</style>  

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

<div style="text-align: center; padding-bottom: 1rem;">
<abbr class="badge" style="background-color:#00369f; margin-left:0.1rem; margin-right:0.1rem; font-size:1.1rem;">ICML 2025</abbr>
<!-- <abbr class="badge" style="background-color:#BF5700; margin-left:0.1rem; margin-right:0.1rem; font-size:1.1rem; width:80px; display:inline-block; text-align:center;">Arxiv</abbr> -->
</div>

<div class="authors"> 
    <a href="https://hychiang.info">Hung-Yueh Chiang</a><sup>1</sup>,
    <a href="https://ccchang.info/">Chi-Chih Chang</a><sup>2</sup>, 
    <a href="https://www.nfrumkin.com/">Natalia Frumkin</a><sup>1</sup>,
    <br>
    <a href="https://people.cs.nycu.edu.tw/~kcw/">Kai-Chiang Wu</a><sup>3</sup>,
    <a href="https://www.mohsaied.com/">Mohamed S. Abdelfattah</a><sup>2</sup>,
    <a href="https://users.ece.utexas.edu/~dianam/">Diana Marculescu</a><sup>1</sup>
</div>
<div class="authors">
    <sup>1</sup>The University of Texas at Austin,
    <sup>2</sup>Cornell University,
    <sup>3</sup>National Yang Ming Chiao Tung University
</div>
<div style="text-align: center; margin-top:12px;">
    <a href="https://arxiv.org/abs/2503.22879"><i class="fa fa-file-pdf-o" style="font-size:24px;color"></i><b> Paper </b></a> &nbsp;
    <a href="https://github.com/enyac-group/Quamba"><i class="fa fa-github" style="font-size:24px;color"></i><b> Code </b></a> &nbsp;
    <a href="https://huggingface.co/ut-enyac"><span style="font-size: 22px;">&#129303;</span><b> Models </b></a>
</div>


<br>
<div style="text-align: center;">
    <p style="font-family: Comic Neue; font-size: 1.4rem;">
    :wrench: Supports <b>W4A8 / W4A16 / W4AX / W8A8</b> for <b>Mamba1</b> and <b>Mamba2</b> <br>
    :small_red_triangle_down: Up to <b>4<span>&#215;</span> memory reduction</b> <br>
    :rocket: <b>13</b> Token-per-second on <b>Orin Nano 8G</b> with <b>Mamba2-8b</b>
    </p>
</div>
<div class="row mt-3">
    <div class="col-sm-12 mt-3 mt-md-0 offset-0">
        {% include figure.html path="assets/img/projects/quamba2/Quamba2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# 4-bit Mamba1 and Mamba2 blocks
- **W4A8**, **W4A16**, **W4AX**, and **W8A8** for both **Mamba1** and **Mamba2**
- **Headto-toe (H2T)** 4/8-bit quantization from the embedding layer, SSM blocks, to the final output layer
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include gif.html path="assets/img/projects/quamba2/quamba2_supports.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# Storage reduction
- Achieve **4** $$\times$$ memory reduction by Head-to-toe (H2T) 4-bit quantization
- Enable deploying Mamba2-8B on **Nano 8G**
<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/quamba2/quamba2_size_2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# End-to-end latency speedup
- Speedup the generation by **3** $$\times$$ on the A5000 GPU
- Run **13** tokens/second on **Nano 8G**
<div class="row mt-4">
    <div class="col-sm mt-3 mt-md-0 offset-0">
        {% include figure.html path="assets/img/projects/quamba2/quamba2_latency_2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# Generalization and robustness
We search **W4A**$$X$$-mixed (the last row in red) to improve the generalization and robustness for low bit-width SSMs. We evaluate low bit-width SSMs on the large multitask dataset MMLU.
<div class="row mt-4">
    <div class="col-sm mt-3 mt-md-0 offset-0">
        {% include figure.html path="assets/img/projects/quamba2/quamba2_searched_2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


# Zero-shot evaluation
<div class="row mt-4">
    <div class="col-sm-10 mt-4 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/quamba2/quamba2_main_table.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>




# Citation
{% raw %}
```latex
@inproceedings{chiang2025quamba2,
  title={Quamba2: A Robust and Scalable Post-training Quantization Framework for Selective State Space Models},
  author={Chiang, Hung-Yueh and Chang, Chi-Chih and Frumkin, Natalia and Wu, Kai-Chiang, Abdelfattah, Mohamed S.  and Marculescu, Diana},
  booktitle={International Conference on Machine Learning (ICML)},
  year={2025}
}
```
{% endraw %}

<br>
# Acknowledgements
This work was supported in part by the ONR Minerva program, NSF CCF Grant No. 2107085, iMAGiNE - the Intelligent Machine Engineering Consortium at UT Austin, UT Cockrell School of Engineering Doctoral Fellowships, NSF Grant No. 2339084, and Taiwan’s NSTC Grant No. 111-2221-E-A49-148-MY3.