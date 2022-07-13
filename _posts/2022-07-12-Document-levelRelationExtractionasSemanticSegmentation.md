---
title: "Document-level Relation Extraction as Semantic Segmentation"
last_modified_at: 2022-07-12T21:52:50-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the DocuNET paper?
    
    > Document-level Relation Extraction as Semantic Segmentation
    > 
    
- What are the main contributions of the DocuNET paper?
    - treat document level relation extraction as a semantic segmentation task
    - take advantage of global dependency among relational triples
    - leverage U-NET inspired semantic segmentation architecture
    
- Document level relation extraction extracts relation information from `multiple sentences`
    
    ![Untitled](/assets/images/Document-levelRelationExtractionasSemanticSegmentation/Untitled.png)
    
- What are some difficulties with document level relation extraction?
    - subject and object entities may not appear in the same sentence
- Previous approaches for document level RE include `graph based and transformer model based`

## Method

- DocuNET leverages a `entity level` relation matrix
    - each cell is a relation type
    
    ![Untitled](/assets/images/Document-levelRelationExtractionasSemanticSegmentation/Untitled 1.png)
    
- DocuNET problem statement: output matrix is `n x n giving each entity relation`
    
    
- DocuNET encodes a document using a transformer with special `entity mention boundary tokens`
    
    
- DocuNET embeds longer documents using dynamic `window pooling`
    - average embeddings of overlapping tokens of different windows\
    
- DocuNET entity-entity relevance vector from similarity method is concatenation of `element wise similarity, cosine similarity and bilinear similarity`
    - based only on embedding for each entity
    - alternative to a content based method
    
    ![Untitled](/assets/images/Document-levelRelationExtractionasSemanticSegmentation/Untitled 2.png)
    
- DocuNET content based strategy leveraging `entity based attention` and the global `document embedding`
    
    ![Untitled](/assets/images/Document-levelRelationExtractionasSemanticSegmentation/Untitled 3.png)
    
- DocuNET UNET architecture
    
    ![Untitled](/assets/images/Document-levelRelationExtractionasSemanticSegmentation/Untitled 4.png)
    

## Results

- DocuNET case study
    
    ![Untitled](/assets/images/Document-levelRelationExtractionasSemanticSegmentation/Untitled 5.png)
    
- DocuNET strong results on Biomedical datasets
    
    ![Untitled](/assets/images/Document-levelRelationExtractionasSemanticSegmentation/Untitled 6.png)
    
- DocuNET results on DocRED
    
    ![Untitled](/assets/images/Document-levelRelationExtractionasSemanticSegmentation/Untitled 7.png)
    

## Reference

```python
@misc{https://doi.org/10.48550/arxiv.2106.03618,
  doi = {10.48550/ARXIV.2106.03618},
  
  url = {https://arxiv.org/abs/2106.03618},
  
  author = {Zhang, Ningyu and Chen, Xiang and Xie, Xin and Deng, Shumin and Tan, Chuanqi and Chen, Mosha and Huang, Fei and Si, Luo and Chen, Huajun},
  
  keywords = {Computation and Language (cs.CL), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Document-level Relation Extraction as Semantic Segmentation},
  
  publisher = {arXiv},
  
  year = {2021},
  
  copyright = {arXiv.org perpetual, non-exclusive license}
}
```