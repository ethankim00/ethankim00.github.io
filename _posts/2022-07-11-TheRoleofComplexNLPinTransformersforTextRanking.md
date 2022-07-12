---
title: "The Role of Complex NLP in Transformers for Text Ranking"
last_modified_at: 2022-07-11T22:01:27-05:00
categories: paper_review
tags:
    - Paper
---
## Introduction

- What is the name of the BOW-BERT paper?
    
    > The Role of Complex NLP in Transformers for Text Ranking?
    > 
    
- What are the main contributions of the BOW-BERT paper?
    - Show that sequence order does not play a strong role in reranking with a BERT cross encoder
    - Explain why it can still outperform BOW approaches

## Method

- BOW-BERT shows that perturbing query and document word order `does not strongly impact results`
    
    ![Untitled](/assets/images/TheRoleofComplexNLPinTransformersforTextRanking/Untitled.png)
    

## Results

- BOW-BERT removing positional embeddings leads to `small drop in MSMARCO performance`
    
    
- BOW-BERT still outperforms `BM25`
    
    **Explanations:**
    
    - query-passage cross-attention
    - richer embeddings that capture word meanings
    based on aggregated context regardless of the word order
    
    ![Untitled](/assets/images/TheRoleofComplexNLPinTransformersforTextRanking/Untitled 1.png)
    
- BERT performance on GLUE tasks with shuffled input shows `contextual encodings must encode some semantics`
    
    ![Untitled](/assets/images/TheRoleofComplexNLPinTransformersforTextRanking/Untitled 2.png)
    

## Conclusions

This is a good, basic research paper into properties of cross-encoders for information retrieval reranking. It would be interesting to repeat the same experiments for Biencoder information retrieval architectures.