---
title: "UNDERSTANDING DIMENSIONAL COLLAPSE IN CONTRASTIVE SELF-SUPERVISED LEARNING"
last_modified_at: 2022-05-10T20:46:05-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the DirectCLR paper?
    
    > UNDERSTANDING DIMENSIONAL COLLAPSE IN CONTRASTIVE SELF-SUPERVISED LEARNING
    > 
    
    Li Jing, Pascal Vincent, Yann LeCun, Yuandong Tian, FAIR
    
- What are the main contributions of the DirectCLR paper?
    - show how contrastive self-supervised learning on images suffers from dimensional collapse
    - mechanisms causing dimensional collapse
        1. Strong augmentation along feature dimension
        2. implicit regularization favoring low rank solutions
    - Contrastive learning objective that outperforms SIMCLR
- Dimensional Collapse is where the embedding vectors end up
spanning a `lower-dimensional subspace` instead of the entire available embedding
space.
    - problem in both contrastive and noncontrastive self-supervised learning methods
    
    ![Untitled](/assets/images/UNDERSTANDINGDIMENSIONALCOLLAPSEINCONTRASTIVESELF-SUPERVISEDLEARNING/Untitled.png)
    

## Method

- DirectCLR theory: dimensional collapse caused by `strong augmentation`
    
    With fixed matrix X (defined in Eqn 6) and strong augmentation such that X has
    negative eigenvalues, the weight matrix W has vanishing singular values.
    
- DirectCLR theory: dimensional collapse caused by `implicit regularization`
    
    
- DirectCLR design: take a fixed `subvector of representations`
    
    Proposition 1. A linear projector weight matrix only needs to be diagonal.
    Proposition 2. A linear projector weight matrix only needs to be low-rank.
    
    ![Untitled](/assets/images/UNDERSTANDINGDIMENSIONALCOLLAPSEINCONTRASTIVESELF-SUPERVISEDLEARNING/Untitled 1.png)
    

## Results

- DirectCLR results: outperfroms `SIMCLR` with no learnable projector
    
    ![Untitled](/assets/images/UNDERSTANDINGDIMENSIONALCOLLAPSEINCONTRASTIVESELF-SUPERVISEDLEARNING/Untitled 2.png)
    

## Reference

```python
@article{https://doi.org/10.48550/arxiv.2110.09348,
  doi = {10.48550/ARXIV.2110.09348},
  
  url = {https://arxiv.org/abs/2110.09348},
  
  author = {Jing, Li and Vincent, Pascal and LeCun, Yann and Tian, Yuandong},
  
  keywords = {Computer Vision and Pattern Recognition (cs.CV), Artificial Intelligence (cs.AI), Machine Learning (cs.LG), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Understanding Dimensional Collapse in Contrastive Self-supervised Learning},
  
  publisher = {arXiv},
  
  year = {2021},
  
  copyright = {Creative Commons Zero v1.0 Universal}
}
```