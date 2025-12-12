---
layout: page
title: ELANA
full_title: "ELANA: A Simple Energy and Latency Analyzer for LLMs"
authors: Hung-Yueh Chiang, Bokun Wang, Diana Marculescu
description: "Energy and Latency Analyzern for LLMs"
img: assets/img/publication_preview/elana.png
importance: 2
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
    <a href="https://hychiang.info">Hung-Yueh Chiang</a>,
    <a href="https://bokun-wang.github.io">Bokun Wang</a>, 
    <a href="https://users.ece.utexas.edu/~dianam/">Diana Marculescu</a>
</div>
<div class="authors">
    The University of Texas at Austin
</div>
<div style="text-align: center; margin-top:12px;">
    <a href="https://arxiv.org/pdf/2512.09946"><i class="fa fa-file-pdf-o" style="font-size:24px;color"></i><b> Report </b></a> &nbsp;
    <a href="https://github.com/enyac-group/Elana/"><i class="fa fa-github" style="font-size:24px;color"></i><b> Code </b></a> &nbsp;
</div>


<br>
<div style="text-align: center;">
<p style="font-family: Comic Neue; font-size: 1.4rem;">
    &#129303; Support models on Hugging Face, including Transformers, SSMs, and hybrid models <br>
    &#128295; Support energy, latency, model size, KV cache size profiling <br>
    &#128269; Support detailed kernel latency profiling <br>
</p>
</div>
<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/elana/elana.png" title="elana logo" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# Compare to Zeus profiling framework
<div class="row">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include gif.html path="assets/img/projects/elana/compare.png" title="compare" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# Hugging Face interface
<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/elana/hf_interface.png" title="hf interface" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

# Profile model size
<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/elana/model_size.png" title="model size" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>



# Profiling results
### Profile models on A6000
<div class="row mt-4">
    <div class="col-sm-10 mt-4 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/elana/A6000.png" title="A6000 results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

### Profile models on Jetson series
<div class="row mt-4">
    <div class="col-sm-10 mt-4 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/elana/jetson.png" title="jetson results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


# Profiling kernels
We use torch profile to generate a json trace file and visualize it with [Perfetto](https://ui.perfetto.dev/). 
<div class="row mt-4">
    <div class="col-sm-10 mt-4 mt-md-0 offset-1">
        {% include figure.html path="assets/img/projects/elana/perfetto_kernel.png" title="kernel results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


# Citation
{% raw %}
```latex
@article{chiang2025elana,
  title = {ELANA: A Simple Energy and Latency Analyzer for LLMs},
  author = {Chiang, Hung-Yueh and Wang, Bokun and Marculescu, Diana},
  journal = {arXiv preprint arXiv:2512.09946},
  year = {2025},
}
```
{% endraw %}

<br>
# Acknowledgements
This work was supported in part by the ONR Minerva program, NSF CCF Grant No. 2107085, iMAGiNE - the Intelligent Machine Engineering Consortium at UT Austin, UT Cockrell School of Engineering Doctoral Fellowships.