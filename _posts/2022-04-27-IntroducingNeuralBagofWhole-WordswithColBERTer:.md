---
title: "Introducing Neural Bag of Whole-Words with ColBERTer"
last_modified_at: 2022-04-27T21:18:50-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the colBERTer paper?
    
    > Introducing Neural Bag of Whole-Words with ColBERTer:
    Contextualized Late Interactions using Enhanced Reduction
    > 
- What are the main contributions of the colBERTer paper?
    - multitask training step
    - use dense index with limited refinement
    - Study different vector dimension sizes
    - efficient retrieval method
    - better OOD generalization than BM25
- colBERTer is based on previous work `colBERT`
    - Omar Khattab and Matei Zaharia. 2020.
    
- What 3 factors contribute to the storage
    1. number of vectors per document
        - not relevant for every architecture
    2. Number of dimensions per vector 
    3. Number of bytes per dimension
- ColBERTer research questions
    - learned score aggregation
    - only one index required for good results
    - performs scales with token vector dimensionalities
- What are the main features of the colBERTer architecture
    - multiple token and dimension reduction techniques
    
    ![Untitled](/assets/images/IntroducingNeuralBagofWhole-WordswithColBERTer:/Untitled.png)
    
- ColBERTer BOW2 method `aggregate representations of subword tokens`
    - also apply a learned token removal gate
    
- ColBERTer scoring combines `CLS and token vector scores`
    - aggregation of the two is learned
    
- Uni-ColBERTer applies an addition linear layer to `reduce token vector dimensionality to 1`
    - each token represented by a scalar score
    - basically back to a BOW representation
- ColBERTer lends itself to `multiple retrieval workflows`
    
    ![Untitled](/assets/images/IntroducingNeuralBagofWhole-WordswithColBERTer:/Untitled 1.png)
    

## Method

## Results

- ColBERTer results: not really compared to other dense `IR methods`
    
    ![Untitled](/assets/images/IntroducingNeuralBagofWhole-WordswithColBERTer:/Untitled 2.png)
    

## Reference

```python
@misc{https://doi.org/10.48550/arxiv.2203.13088,
  doi = {10.48550/ARXIV.2203.13088},
  
  url = {https://arxiv.org/abs/2203.13088},
  
  author = {Hofstätter, Sebastian and Khattab, Omar and Althammer, Sophia and Sertkan, Mete and Hanbury, Allan},
  
  keywords = {Information Retrieval (cs.IR), Artificial Intelligence (cs.AI), Computation and Language (cs.CL), Machine Learning (cs.LG), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Introducing Neural Bag of Whole-Words with ColBERTer: Contextualized Late Interactions using Enhanced Reduction},
  
  publisher = {arXiv},
  
  year = {2022},
  
  copyright = {arXiv.org perpetual, non-exclusive license}
}
```