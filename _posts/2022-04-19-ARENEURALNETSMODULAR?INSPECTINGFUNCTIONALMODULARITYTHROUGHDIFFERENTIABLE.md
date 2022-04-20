---
title: "ARE NEURAL NETS MODULAR? INSPECTING FUNCTIONAL MODULARITY THROUGH DIFFERENTIABLE"
last_modified_at: 2022-04-19T20:34:36-05:00
categories: paper_review
tags:
    - Machine Learning
---

machinelearning::papers::AreNeuralNetworksModular

## Introduction

- What is the name of the Modular Neural Nets paper?
    
    > ARE NEURAL NETS MODULAR? INSPECTING FUNCTIONAL MODULARITY THROUGH DIFFERENTIABLE
    WEIGHT MASKS
    > 
    
    Jürgen Schmidhuber
    
- What are the main contributions of the modular neural nets paper?
    - binary differentiable weight mask tool for probing model modularity
    - define metrics to measure degree of model modularity P specialize and P reuse

## Method

- Modular Neural networks probes submodules using a `differentiable weight mask`
    - formulate task corresponding to ability that we want to probe
    - train the weight mask on this task while keeping the original model weights frozen
    - masks are binary which only allows for removing individual weights
- Modular Neural Networks trains binarized masks using the `Gumbel Softmax estimator`
    - weights are initialized to have a high probability of being masked
- Modular Neural Networks analyzes the overlap between two tasks using two metrics
    1. Intersection over Union: degree of overlap of activated weights
    2. Intersection over minimum: number of shared weights over minimum number of weights for one task
- One motivation for having high P specialize in a model is to `prevent catastrophic forgetting`
    - modifying one submodule should not effect the other modules
- P reused is desirable for a model in order to help with `data efficiency and generalization`

## Results

- Modular Neural Networks conclusion: networks have `specialized submodules` but tend `not to reuse them`
    
    
- Double Addition task results: show the `modules are not reused`
    - binary mask for one addition not reused in the other
    
    ![Untitled](/assets/images/ARENEURALNETSMODULAR?INSPECTINGFUNCTIONALMODULARITYTHROUGHDIFFERENTIABLE/Untitled.png)
    
- Theoretical explanations for lack of P_reuse in LSTM language models is that `data routing is hard for learned weights to simulate`

## Conclusions

Overall this paper’s approach and conclusion demonstrate that Neural Networks are not in fact modular. The idea of training a differentiable binary weight mask is simple and interesting. However I wonder if there is a better way to explore modularity of neural networks using a more structured grouping of the model parameters (rather than treating each parameter individually).

## Reference

```python
@article{DBLP:journals/corr/abs-2010-02066,
  author    = {R{\'{o}}bert Csord{\'{a}}s and
               Sjoerd van Steenkiste and
               J{\"{u}}rgen Schmidhuber},
  title     = {Are Neural Nets Modular? Inspecting Functional Modularity Through
               Differentiable Weight Masks},
  journal   = {CoRR},
  volume    = {abs/2010.02066},
  year      = {2020},
  url       = {https://arxiv.org/abs/2010.02066},
  eprinttype = {arXiv},
  eprint    = {2010.02066},
  timestamp = {Mon, 12 Oct 2020 17:53:10 +0200},
  biburl    = {https://dblp.org/rec/journals/corr/abs-2010-02066.bib},
  bibsource = {dblp computer science bibliography, https://dblp.org}
}
```