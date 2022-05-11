---
title: "Embedding Hallucination for Few-Shot Language Fine-tuning"
last_modified_at: 2022-05-10T20:50:44-05:00
categories: paper_review
tags:
    - Machine Learning
---


## Introduction

- What is the name of the EmbedHalluc paper?
    
    > Embedding Hallucination for Few-Shot Language Fine-tuning
    > 
    
    [Yiren Jian](/assets/images/EmbeddingHallucinationforFew-ShotLanguageFine-tuninghttps://arxiv.org/search/cs?searchtype=author&query=Jian%2C+Y)
    , [Chongyang Gao](/assets/images/EmbeddingHallucinationforFew-ShotLanguageFine-tuninghttps://arxiv.org/search/cs?searchtype=author&query=Gao%2C+C)
    , [Soroush Vosoughi](/assets/images/EmbeddingHallucinationforFew-ShotLanguageFine-tuninghttps://arxiv.org/search/cs?searchtype=author&query=Vosoughi%2C+S)
    
    - Same authors as SupCON paper
- What are the main contributions of the EmbedHalluc paper?
    - cWGAN based augmentation in the embedding space
- The Wasserstein GAN uses `Wasserstein` distance as the objective function to stabilize the training of GAN.

## Method

- Hallucinated embedding in HallucEmbed are `pseudolabeled by a teacher model`
    
    
- HallucEmbed trains a GAN discriminator to distinguish between `real and hallucinated embeddings`
    
    ![Untitled](/assets/images/EmbeddingHallucinationforFew-ShotLanguageFine-tuning/Untitled.png)
    

## Results

- EmbedHalluc roBERTa large results `pretty good` few shot performance
    - not as good as my DE method
    
    ![Untitled](/assets/images/EmbeddingHallucinationforFew-ShotLanguageFine-tuning/Untitled 1.png)
    

## Reference

```python
@misc{https://doi.org/10.48550/arxiv.2205.01307,
  doi = {10.48550/ARXIV.2205.01307},
  
  url = {https://arxiv.org/abs/2205.01307},
  
  author = {Jian, Yiren and Gao, Chongyang and Vosoughi, Soroush},
  
  keywords = {Computation and Language (cs.CL), Artificial Intelligence (cs.AI), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Embedding Hallucination for Few-Shot Language Fine-tuning},
  
  publisher = {arXiv},
  
  year = {2022},
  
  copyright = {arXiv.org perpetual, non-exclusive license}
}
```