---
layout: post
title: "Minions: Cost-efficient Collaboration Between On-device and Cloud Language Models"
date: 2025-03-16
description: "Minions: Cost-efficient Collaboration Between On-device and Cloud Language Models"
tags: ml paper llm federated distributed
comments: true
---

<style>  
li {  
    font-size: 1.1em; /* Adjust as needed */  
}  
</style>  

# [Minions: Cost-efficient Collaboration Between On-device and Cloud Language Models](https://arxiv.org/abs/2502.15964)  
> [TL;DR]  
> MinionS is a collaboration protocol between **local small LMs** and **remote frontier LMs**, significantly reducing cloud inference costs while maintaining near-frontier accuracy by decomposing complex tasks into simpler subtasks executed locally in parallel.


## Highlights  
- Proposes a collaboration protocol between **local small LMs** and **remote frontier LMs**
- Proposes two protocols: Minion (naïve chat-based) and MinionS (task decomposition-based)
- MinionS reduces cloud inference costs by **5.7×** on average and recovers **97.9%** of frontier LM performance
- Conducts detailed analyses on model choice, parallel workload scaling, and sequential communication strategies

## Summary 
- **Observation 1**: Large model can perform data-intensive reasoning, but accessing these models is expensive.
- **Observation 2**: Small local models run on-device with nocost, but they struggle with multi-step instructions and long context reasoning.
<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 offset-3">
        {% include figure.html path="assets/img/posts/minions/slm.png" title="Small language models" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>
- **The problem statement**: Reducing the cost of cloud-based inference while maintaining performance by enabling effective collaboration between small, on-device LMs and large, remote LMs.
- **The solution**: MinionS leverages the remote LM to decompose complex queries into simpler subtasks which are executed in parallel by local LMs on smaller document chunks, improving accuracy and reducing remote inference costs.
{% include figure.html path="assets/img/posts/minions/overview.png" title="Minions Overview" class="img-fluid rounded z-depth-1" %} 
- **Finding 1**: Practical Effectiveness: Achieves near-equivalent accuracy to large remote-only models at just a fraction (around 18%) of the cost.
- **Finding 2**: Effective collaboration is achievable starting from a 3B-parameter local model, with larger local models (8B) further improving accuracy and cost efficiency.
<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/posts/minions/slm_size.png" title="Small language models size" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


## Experiments  

#### Comparison with Baselines
- Minion (naïve protocol) achieves 30.4× cost reduction but with 87% of the accuracy of the remote-only model.
- MinionS substantially improves on Minion by achieving 97.9% of the accuracy at only 18% of the cost compared to remote-only inference.
<div class="row mt-3">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/posts/minions/baselines.png" title="Minions baselines" class="img-fluid rounded z-depth-1" %} 
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/posts/minions/table.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


#### Analysis of Parallel Workloads
- Three parameters configured by RemoteLM for increasing the degree of task decomposition:
  - (1) **Number of tasks per round**: How many simpler jobs or subtasks the remote model creates for the local models, enabling parallel execution locally. (i.e. “Extract the ARR for Q1 of 2014”)
  - (2) **Number of samples per task**: Number of repeated attempts (samples) made by the local language model (LocalLM) for each individual subtask (i.e. number of generations created with LocalLM, ≥ 1).
  - (3) **Chunk size**: chunk by page, chunk by paragraph, etc; smaller chunks will send more information to cloud.

- Increasing the number of tasks, samples per task, and chunking granularity improves accuracy but increases cost. Task decomposition and chunk size offer a more cost-effective trade-off.

<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/posts/minions/scaling.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


#### Sequential Communication
- Increasing sequential communication rounds can improve accuracy, but also increases cost.
- Strategies like using a scratchpad for intermediate steps slightly improve the cost-accuracy tradeoff.
<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 offset-3">
        {% include figure.html path="assets/img/posts/minions/sequential.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>

#### Retrieval-Augmented Generation (RAG) Comparison
- RAG excels in structured extraction tasks but struggles with tasks requiring synthesis from dispersed information, where MinionS provides superior token efficiency and narrative coherence.
- From Figure 8 left, none of the RAG configurations are are able to match the quality of Minion at the same low cost.
<div class="row mt-3">
    <div class="col-sm-10 mt-3 mt-md-0 offset-1">
        {% include figure.html path="assets/img/posts/minions/rag.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<br>


## Conclusions

- MinionS efficiently distributes workload between local and remote LMs, significantly reducing cloud inference costs while preserving accuracy.
- This collaboration approach becomes increasingly effective with advancements in local LM capabilities, showing strong potential for future cost-efficient systems.

