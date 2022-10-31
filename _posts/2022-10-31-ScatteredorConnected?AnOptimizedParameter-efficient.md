---
title: "Scattered or Connected? An Optimized Parameter-efficient"
last_modified_at: 2022-10-31T18:39:43-05:00
categories: paper_review
tags:
    - : Reading
---
Type: Paper

## Introduction

- What is the name of the IAA paper?
    
    > Scattered or Connected? An Optimized Parameter-efficient
    Tuning Approach for Information Retrieval
    > 
    
- What are the main contributions of the IAA paper?
    1. Show how parameter efficient methods for IR are not learning efficient
        1. ie lower performance or convergence
    2. Theoretical reasons for learning innefficiency
    3. Insertion method to smooth loss landscape and 

## Method

- IAA biencoder results: lags `full finetuning` on many datasets
    
    ![Untitled](/assets/images/ScatteredorConnected?AnOptimizedParameter-efficient/Untitled.png)
    
- IAA cross encoder results: slightly lags `full finetuning`
    
    ![Untitled](/assets/images/ScatteredorConnected?AnOptimizedParameter-efficient/Untitled 1.png)
    

- IAA proposes to improve parameter efficient methods by `adding skip connections to improve gradient flow`
    - “aside” module
    - bottleneck architecture
    
    ![Untitled](/assets/images/ScatteredorConnected?AnOptimizedParameter-efficient/Untitled 2.png)
    
- What are the three IAA variants?
    - different ways of inserting bottleneck layers
    
    ![Untitled](/assets/images/ScatteredorConnected?AnOptimizedParameter-efficient/Untitled 3.png)
    

## Results

- IAA BN method biencoder results: gives `tiny improvement`
    
    ![Untitled](/assets/images/ScatteredorConnected?AnOptimizedParameter-efficient/Untitled 4.png)
    
- IAA result summary
    - better match or exceed full finetuning performance
    - faster convergence
    - L adapter works best
    - Doesn’t beat methods with more advanced hard negative mining and data augmentation

## Conclusions

This paper presents some good theoretical analysis and. It’s clear that if such results were to hold for regimes outside of small encoder models on information retrieval datasets, it would have the potential to make PET vastly more competitive. 

## Reference

```python
@inproceedings{Ma_2022,
	doi = {10.1145/3511808.3557445},
  
	url = {https://doi.org/10.1145%2F3511808.3557445},
  
	year = 2022,
	month = {oct},
  
	publisher = {{ACM}
},
  
	author = {Xinyu Ma and Jiafeng Guo and Ruqing Zhang and Yixing Fan and Xueqi Cheng},
  
	title = {Scattered or Connected? An Optimized Parameter-efficient Tuning Approach for Information Retrieval},
  
	booktitle = {Proceedings of the 31st {ACM} International Conference on Information and Knowledge Management}
}
```