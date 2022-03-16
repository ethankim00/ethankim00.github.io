---
title: "UCTopic: Unsupervised Contrastive Learning for Phrase Representations"
last_modified_at: 2022-03-15T21:52:58-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

This paper leverages a new contrastive pretraining objective along with cluster assisted contrastive learning to generate high-quality phrase/entity embeddings. 

- What is the name of the UCTopic paper?
    
    > UCTopic: Unsupervised Contrastive Learning for Phrase Representations
    and Topic Mining
    > 
    
    Jiacheng Li, Jingbo Shang, Julian McAuley, UCSD
    
- UCTopic makes two assumptions about phrase semantics
    
    1) Phrase semantics are determined by their contexts
    
    2) Phrases that have the same mentions have the same semantics
    
    ![Untitled](/assets/images/UCTopic:UnsupervisedContrastiveLearningforPhraseRepresentations/Untitled.png)
    
- Why does UCTopic have to leverage clusters?
    - small number of topics makes in batch negatives unstable
    
- What are the main contributions of the UCTopic paper
    - Unsupervised contrastive learning method for phrase
    - cluster assisted negative sampling method
    - achieve superior results on entity clustering and topic mining

## Method

- UCTopic uses `LUKE` as a backbone model
    - transformer based model pretrained for good entity embedding
    - When using LUKE as an encoder enter the sentence as well as the left and right boundaries of the phrase of interest
- LUKE is a roBERTa model that is optimized for `entity embeddings`
- UCTopic contrastive objective: positive samples are `different contexts for the same phrase`
    - ex: “He lived on the east coast of the
    United States” and “How much does it cost to fly to
    the United States”.
    - mask out phrase “UNITED STATES” and use as positive instances
    - use in batch negatives
    
    ![Untitled](/assets/images/UCTopic:UnsupervisedContrastiveLearningforPhraseRepresentations/Untitled 1.png)
    
- The UCTopic dataset is English wikipedia with `hyperlinks` as phrases
    - Using wikipedia hyperlinks is a good source of meaningful entities
    
- UCTopic finetuning is done with `cluster assisted negatives`
    - compute clusters of phrase representations based on pretrained UCTopic model
    
    ![Untitled](/assets/images/UCTopic:UnsupervisedContrastiveLearningforPhraseRepresentations/Untitled 2.png)
    
- The intuition behind cluster assisted negative sampling in UCTopic is to `mine more different phrases as negative samples`

## Results

- UCTopic evaluation is done on entity clustering showing that model learns `good contextual embeddings of entity phrases`
    - Identify Person, Location and Organization entities
    - comparable performance to phrase BERT
    
    ![Untitled](/assets/images/UCTopic:UnsupervisedContrastiveLearningforPhraseRepresentations/Untitled 3.png)
    

- How is UCTopic applied to topic mining?
    - mine 10k phrases from dataset
    - do Kmeans clustering on UCTopic phrase representations
        - choose number of clusters using Silhouette Score
    - Finetune UCTopic with CCL
    - 
- UCTopic can be used to generate `more lexically diverse` topic phrases
    
    ![Untitled](/assets/images/UCTopic:UnsupervisedContrastiveLearningforPhraseRepresentations/Untitled 4.png)
    
- How does UCTopic evaluate the results of topical phrase mining?
    
    1) Topical Separation
    
    - human evaluation: **phrase intrusion task**
    
    2) Phrase Coherence
    
    - based on annotator judgement
    
    3) Phrase informativeness and diversity
    
    - use tfidf
    - ratio of distinct words among top phrases
    
- UCTopic precision at n metric measures if `top phrases within a topic reflect that topic well`

## Conclusions

It’s interesting to see that clustering on high-quality phrase representations gives good topics. Topic modeling is a niche but important application of short phrase representations. However, as evidenced by the convoluted approaches in this paper, evaluation of topic mining methods is pretty difficult. Overall I find the information retrieval results more compelling and UCTopic outperforms a strong alternate phrase embedding method: PhraseBERT.

## Reference

```python
@article{li2022uctopic,
title={UCTopic: Unsupervised Contrastive Learning for Phrase Representations
  and Topic Mining },
author={Jiacheng Li and Jingbo Shang and Julian McAuley },
year={2022},
journal={arXiv preprint arXiv: Arxiv-2202.13469}
}
```