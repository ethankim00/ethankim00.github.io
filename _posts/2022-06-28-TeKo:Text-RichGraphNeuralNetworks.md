---
title: "TeKo: Text-Rich Graph Neural Networks"
last_modified_at: 2022-06-28T22:25:11-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the TEKO paper?
    
    > TeKo: Text-Rich Graph Neural Networks
    with External Knowledge
    > 
- What are the main contributions of the TeKo paper?
    - use external knowledge from triplets and entity descriptions to inform network representations
    - SOTA results
    
- Examples of text rich networks include DBLP and `ecommerce networks`
    - DBLP document nodes have text of abstracts
- Example of a text rich network incorporating entity information
    - entities can be identified using TagMe and looked up in an external database
    
    ![Untitled](/assets/images/TeKo:Text-RichGraphNeuralNetworks/Untitled.png)
    

## Method

- The TeKo architecture consists of 3 main components: `semantic network generation` knowledge based entity representation and heterogeneous graph attention
    
    ![Untitled](/assets/images/TeKo:Text-RichGraphNeuralNetworks/Untitled 1.png)
    
- TeKo builds an entity network by `getting entities from tagME above certain threshold and defining adjacency matrix with thresholded cosine similarity between knowledge based entity representations`
    
    
- The TeKo heterogenous graph contains `document nodes and entity nodes`
    - document-document edges: eg citation relationships
    - entity-entity edges: cosine similarity
    - document-entity edges: inclusion relationships
    
- TransE is a method to embed entities given `knowledge graph triplet information`
    
    
- TransE seeks to embed entities such that `sum of head and relation embeddings equals tail embedding`
    
    ![Untitled](/assets/images/TeKo:Text-RichGraphNeuralNetworks/Untitled 2.png)
    
- TeKo uses a `learned gating function` to aggregate entity information from knowledge graph and textual descriptions
    
    

## Results

- TeKO method does well on `node classification benchmarks`
    
    ![Untitled](/assets/images/TeKo:Text-RichGraphNeuralNetworks/Untitled 3.png)
    
- TeKo was also applied to `relevance matching for ecommerce queries`
    - leverage some external category info
    
    ![Untitled](/assets/images/TeKo:Text-RichGraphNeuralNetworks/Untitled 4.png)
    

## Conclusions

- Could the learned representations be applied for entity based document retrieval?

## Reference

```python
@article{yu2022teko,
  title={TeKo: Text-Rich Graph Neural Networks with External Knowledge},
  author={Yu, Zhizhi and Jin, Di and Wei, Jianguo and Liu, Ziyang and Shang, Yue and Xiao, Yun and Han, Jiawei and Wu, Lingfei},
  journal={arXiv preprint arXiv:2206.07253},
  year={2022}
}
```