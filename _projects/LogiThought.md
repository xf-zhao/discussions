---
layout: page
title: LogiThought
description: Enhancing Zero-Shot Chain-of-Thought Reasoning in Large Language Models through Logic
img: /assets/img/publication_preview/LogiThought-frontpage.png
importance: 1
category: work
related_publications: Zhao23EnhancingZeroShot
giscus_comments: true
---

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="https://logi-cot.github.io/img/LogiCoT_preview.gif" title="LogiThought process" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Figure 1. Logical CoT correctifies CoT and results in correct answer.
</div>

## Abstract
Recent advancements in large language models have showcased their remarkable generalizability across various domains. However, their reasoning abilities still have significant room for improvement, especially when confronted with scenarios requiring multi-step reasoning. Although large language models possess extensive knowledge, their behavior, particularly in terms of reasoning, often fails to effectively utilize this knowledge to establish a coherent thinking paradigm. Generative language models sometimes show hallucinations as their reasoning procedures are unconstrained by logical principles. Aiming to improve the zero-shot chain-of-thought reasoning ability of large language models, we propose Logical Chain-of-Thought (LogiCoT), a neurosymbolic framework that leverages principles from symbolic logic to verify and revise the reasoning processes accordingly. Experimental evaluations conducted on language tasks in diverse domains, including arithmetic, commonsense, symbolic, causal inference, and social problems, demonstrate the efficacy of the enhanced reasoning paradigm by logic.

## Methodology

## Citation
```text
@misc{Zhao23EnhancingZeroShot,
  title = {Enhancing {{Zero-Shot Chain-of-Thought Reasoning}} in {{Large Language Models}} through {{Logic}}},
  author = {Zhao, Xufeng and Li, Mengdi and Lu, Wenhao and Weber, Cornelius and Lee, Jae Hee and Chu, Kun and Wermter, Stefan},
  year = {2023},
  month = sep,
  number = {arXiv:2309.13339},
  eprint = {2309.13339},
  primaryclass = {cs},
  publisher = {{arXiv}},
  doi = {10.48550/arXiv.2309.13339},
  urldate = {2023-09-26},
  abstract = {Recent advancements in large language models have showcased their remarkable generalizability across various domains. However, their reasoning abilities still have significant room for improvement, especially when confronted with scenarios requiring multi-step reasoning. Although large language models possess extensive knowledge, their behavior, particularly in terms of reasoning, often fails to effectively utilize this knowledge to establish a coherent thinking paradigm. Generative language models sometimes show hallucinations as their reasoning procedures are unconstrained by logical principles. Aiming to improve the zero-shot chain-of-thought reasoning ability of large language models, we propose Logical Chain-of-Thought (LogiCoT), a neurosymbolic framework that leverages principles from symbolic logic to verify and revise the reasoning processes accordingly. Experimental evaluations conducted on language tasks in diverse domains, including arithmetic, commonsense, symbolic, causal inference, and social problems, demonstrate the efficacy of the enhanced reasoning paradigm by logic.},
  archiveprefix = {arxiv},
  arxiv = {2309.13339},
}
```