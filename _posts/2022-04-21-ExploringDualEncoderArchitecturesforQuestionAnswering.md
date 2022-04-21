---
title: "Exploring Dual Encoder Architectures for Question Answering"
last_modified_at: 2022-04-21T18:52:43-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the Dual Encoder Question Answering paper?
    
    > Exploring Dual Encoder Architectures for Question Answering
    > 
    
    google
    
- What are the main contributions of the Dual Encoder Question Answering paper?
    - show superior performance of SDEs over ADEs
    - improvement of ADEs by sharing projection layer
- In an Assymetric Dual Encoder parameters are not shared between the `query and passage encoder`
    - example: DPR
    - generally worse than Siamese Dual Encoder

## Method

- For Dual Encoders there are several options for parameter sharing for example can `share input embedding matrices`
    
    ![Untitled](/assets/images/ExploringDualEncoderArchitecturesforQuestionAnswering/Untitled.png)
    

## Results

- Assymetric Dual Encoders fail to `separate` query and passage in the embedding space
    
    ![Untitled](/assets/images/ExploringDualEncoderArchitecturesforQuestionAnswering/Untitled 1.png)
    
- Best results for Assymetric Dual Encoders are achieved with `shared output projection weights`
    
    ![Untitled](/assets/images/ExploringDualEncoderArchitecturesforQuestionAnswering/Untitled 2.png)
    

## Conclusions

This is a nice paper giving some empirical insight into different retrieval techniques. It’s a little disappointing that assymetric dual encoders can’t perform better given that there is often a much different data distribution captured by the query and passages in an information retrieval system. 

## Reference

```python
@article{dong2022exploring,
  title={Exploring Dual Encoder Architectures for Question Answering},
  author={Dong, Zhe and Ni, Jianmo and Bikel, Dan and Alfonseca, Enrique and Wang, Yuan and Qu, Chen and Zitouni, Imed},
  journal={arXiv preprint arXiv:2204.07120},
  year={2022}
}
```