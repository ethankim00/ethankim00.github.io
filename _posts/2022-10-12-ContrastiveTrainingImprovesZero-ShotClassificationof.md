---
title: "Contrastive Training Improves Zero-Shot Classification of"
last_modified_at: 2022-10-12T20:36:40-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the LayoutBERT paper?
    
    > Contrastive Training Improves Zero-Shot Classification of
    Semi-structured Documents
    > 
    
    AWS AI Labs
    
- What are the main contributions of the LayoutBERT paper?
    - explore the zero shot classification setting
    - pairwise contrastive objective for pretraining and finetuning
    - unsupervised pretraining with pseudolabels

## Method

- LayoutBERT pseudolables are `subset of tokens extracted from a document`
    
    ![Untitled](/assets/images/ContrastiveTrainingImprovesZero-ShotClassificationof/Untitled.png)
    
- LayoutBERT uses two encoders, a text encoder and a `document encoder` that incorporates visual information
- The LayoutBERT architecture encodes `text and 2D positional embeddings`
    - like LayoutLM with fewer positional embeddings and no visual information
- LayoutBERT finetuning is done with `the class labels as the text`

## Results

- LayoutBERT zero shot performance results
    
    ![Untitled](/assets/images/ContrastiveTrainingImprovesZero-ShotClassificationof/Untitled 1.png)
    
- LayoutBERT supervised zero shot performance results: `contrastive finetuning does the best`
    
    ![Untitled](/assets/images/ContrastiveTrainingImprovesZero-ShotClassificationof/Untitled 2.png)
    

## Conclusions

This paper provides a simple boost to baseline methods for structured document classification. It’s clear that the benefit of the method comes from the information included in the text descriptions of the labels. I would have liked to see the label content analyzed for the datasets that are evaluated. 

## Reference

```python
https://arxiv.org/pdf/2210.05613.pdf
```