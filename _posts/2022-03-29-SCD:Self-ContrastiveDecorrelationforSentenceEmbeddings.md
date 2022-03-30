---
title: "SCD: Self-Contrastive Decorrelation for Sentence Embeddings"
last_modified_at: 2022-03-29T22:33:07-05:00
categories: paper_review
tags:
    - Paper
---
## Introduction

- What is the name of the SCD paper?
    
    > SCD: Self-Contrastive Decorrelation for Sentence Embeddings
    > 
    
    Tassilo Klein, Moin Nabi, SAP AI research
    
- What are the main contributions of the SCD paper?
    - sentence embedding method leveraging multidropout without negative pairs
    - novel feature decorrelation objective for non contrastive self-supervised learning
    
- What are the disadvantages of using negative pairs in contrastive learning?
    - requirement to carefully construct negative pairs (eg UCTOPIC)
        - ie hard negative mining may be difficult
    - rely on large batch sizes
        - large batch size is more likely to have hard negatives
    - rely on complicated memory strategies
- SCD is clearly analogous to BYOL while SIMCSE is clearly inspired by SIMCLR
    - exciting to see transfer of self supervised learning methods from the vision domain to the text domain

## Method

- SCD generates multiple augmentations for each sample by `varying the dropout rate`
    - generate one embedding with high dropout and one with low dropout
    
- In SCD self contrastive loss is applied to the `embeddings` while feature decorrelation is applied to `high dimensional feature space`
    - embeddings are projected to high dimensional feature space by MLP
    
    ![Untitled](/assets/images/SCD:Self-ContrastiveDecorrelationforSentenceEmbeddings/Untitled.png)
    
- SCD self contrastive divergence objective seeks to generate `contrast between embeddings from different dropouts`
    - analogous to pushing about of negative sample in contrastive learning
    - Loss is cosine similarity of embeddings
    
    ![Untitled](/assets/images/SCD:Self-ContrastiveDecorrelationforSentenceEmbeddings/Untitled 1.png)
    
- The SCD feature decorrelation objective seeks to make embeddings `invariant to augmentation`
    - analogous to pushing positive samples closer together
    - They project embedding to a higher dimensional space: MLP $p()$
    - calculate a correlation matrix:
- SCD calculates a correlation matrix using `multiple embeddings from same batch`
    - dimension is minibatch size x minibatch size
    
    ![Untitled](/assets/images/SCD:Self-ContrastiveDecorrelationforSentenceEmbeddings/Untitled 2.png)
    
- The SCD feature decorrelation objective maximizes cross correlation along the `diagonal`
    - diagonal represents positive pair
    - off diagonals represent correlation between negative pairs
    
    ![Untitled](/assets/images/SCD:Self-ContrastiveDecorrelationforSentenceEmbeddings/Untitled 3.png)
    

## Results

- SCD experimental details
    - Train on 100k randomly sampled sentences from wikipedia
    - projector MLP has 3 layers
    
- SCP results on `STS` lag behind SIMCSE
    - note: also behind promptBERT and DCPCSE
    
    ![Untitled](/assets/images/SCD:Self-ContrastiveDecorrelationforSentenceEmbeddings/Untitled 4.png)
    
- SCP results on downstream tasks: slight improvement
    - senteval with logistic regression classifier
    - beats SimCSE
    
    ![Untitled](/assets/images/SCD:Self-ContrastiveDecorrelationforSentenceEmbeddings/Untitled 5.png)
    
- SCD ablation study: best results from `both objectives combined`
    - note, it’s pretty intuitive that the first loss alone would have very bad performance
    
    ![Untitled](/assets/images/SCD:Self-ContrastiveDecorrelationforSentenceEmbeddings/Untitled 6.png)
    

## Conclusions

This paper presents an alternative to contrastive methods for learning of sentence embeddings. Two competing objectives are used. One leveraging self-contrast to promote augmented samples with different dropout masks to be different from each other. The second seeks to promote invariance of a sample to the augmentation applied. Given that in the decorrelation objective, the off diagonal entries of the correlation matrix can be thought of as implicitly deriving from negative samples. Therefore the claimed advantage of this method is rather suspect. Also they state that a disadvantage of contrastive learning is the need for higher batch sizes. However, the SIMCSE paper did find that their method was insensitive to the batch size. Overall it’s interesting to see how augmented sample can be treated as positive and negative samples simultaneously. I would like to see follow up research develop more principles ways to leverage these two, seemingly contradictory objectives.

## Reference

```python
@article{klein2022scd,
  title={SCD: Self-Contrastive Decorrelation for Sentence Embeddings},
  author={Klein, Tassilo and Nabi, Moin},
  journal={arXiv preprint arXiv:2203.07847},
  year={2022}
}
```