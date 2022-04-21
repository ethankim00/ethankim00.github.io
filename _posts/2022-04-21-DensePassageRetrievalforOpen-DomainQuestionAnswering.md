---
title: "Dense Passage Retrieval for Open-Domain Question Answering"
last_modified_at: 2022-04-21T18:51:44-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the DPR paper?
    
    > Dense Passage Retrieval for Open-Domain Question Answering
    > 
    
    FAIR
    
- What are the main contributions of the DPR paper?
    - original dense retrieval outperforms TFIDF/BM25
    - simple finetuning can improve dense retrieval
    - higher retrieval precision leads to higher QA accuracy
- What 2 weaknesses with the ORQA approach does the DPR paper identify?
    1. ICT pretraining is computationally expensive
    2. Context encoder is not finetuned

## Method

- The DPR method uses `separate BERT encoders` for query and passages
    - uses the dot product between them
    
- The DPR objective is basically `NCE loss`
    - they use in batch negatives
    
    ![Untitled](/assets/images/DensePassageRetrievalforOpen-DomainQuestionAnswering/Untitled.png)
    
- DPR training considers `3` different types of negatives
    1. Random: any random passage
    2. BM25: top passages returned by BM25 that do not match question (more difficult set of negatives to predict)
    3. GOLD: positive passages paired with other questions in the training set

## Results

- DPR results: outperforms `BM25`
    
    ![Untitled](/assets/images/DensePassageRetrievalforOpen-DomainQuestionAnswering/Untitled 1.png)
    
- DPR results performance scales with `number of training passages`
    
    ![Untitled](/assets/images/DensePassageRetrievalforOpen-DomainQuestionAnswering/Untitled 2.png)
    
- DPR results `dot product` and L2 distance outperform cosine similarity
    - dot product is cheapest computationally

## Conclusions

This was a basic paper on using BERT for dense passage retrieval. Notable design decisions were having a separate query and passage encoder and using dot product similarity.

## Reference

```python
**@article{karpukhin2020dense,
  title={Dense passage retrieval for open-domain question answering},
  author={Karpukhin, Vladimir and O{\u{g}}uz, Barlas and Min, Sewon and Lewis, Patrick and Wu, Ledell and Edunov, Sergey and Chen, Danqi and Yih, Wen-tau},
  journal={arXiv preprint arXiv:2004.04906},
  year={2020}
}**
```