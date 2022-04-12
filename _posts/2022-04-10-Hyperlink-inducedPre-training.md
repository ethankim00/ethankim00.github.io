---
title: "Hyperlink-induced Pre-training"
last_modified_at: 2022-04-10T21:48:38-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the Hyperlink-induced Pre-training paper?
    
    > Hyperlink-induced Pre-training
    for Passage Retrieval in Open-domain Question Answering
    > 
- What are the main contributions of the Hyperlink-induced Pre-training
paper?
    - Hyperlink method improves performance on Open Domain QA

## Method

- QP relevant pairs are passages which provide evidence relating to a query
    - pairs can be mined from wikipedia passages with a link to each other
- Two kinds of hyperlink topology for relevance construction are `dual link and comention`
    
    **Dual Link:** passage pairs link to each other
    
    **Co-mention:** passage pairs that both link to the same third passage
    
    ![Untitled](/assets/images/Hyperlink-inducedPre-training/Untitled.png)
    
- Hyperlink Induced Pretraining trains on `positive QP pairs` with in batch negative sampling
    
    ![Untitled](/assets/images/Hyperlink-inducedPre-training/Untitled 1.png)
    

## Results

- Hyperlink Induced Pretraining gets superior performance on QA tasks
    - measurement is of retrieval accuracy
    
    ![Untitled](/assets/images/Hyperlink-inducedPre-training/Untitled 2.png)
    
- Hyperlink Induced Pretraining generalization to MSMARCO retrieval `doesn't necessarily generalize OOD`
    
    ![Untitled](/assets/images/Hyperlink-inducedPre-training/Untitled 3.png)
    

## Conclusions

In one sentence this paper could be summarized as contrastive training with linked passages as positive pairs. Despite the complicated notation masking this extreme simplicity, the paper achieves good results on retrieval for OpenDomain QA. Notably, this paper is very similar in concept to the LinkBERT paper. However, this paper takes a simpler approach that doesn’t require modifications to the pretraining scheme. It would be interesting to find additional data sources with hyperlinks that can be exploited to better train language models.

## Reference

```python
@misc{https://doi.org/10.48550/arxiv.2203.06942,
  doi = {10.48550/ARXIV.2203.06942},
  
  url = {https://arxiv.org/abs/2203.06942},
  
  author = {Zhou, Jiawei and Li, Xiaoguang and Shang, Lifeng and Luo, Lan and Zhan, Ke and Hu, Enrui and Zhang, Xinyu and Jiang, Hao and Cao, Zhao and Yu, Fan and Jiang, Xin and Liu, Qun and Chen, Lei},
  
  keywords = {Computation and Language (cs.CL), Information Retrieval (cs.IR), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Hyperlink-induced Pre-training for Passage Retrieval in Open-domain Question Answering},
  
  publisher = {arXiv},
  
  year = {2022},
  
  copyright = {Creative Commons Attribution 4.0 International}
}
```