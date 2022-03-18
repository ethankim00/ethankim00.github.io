---
title: "Visualizing and Measuring the Geometry of BERT"
last_modified_at: 2022-03-17T20:24:35-05:00
categories: paper_review
tags:
    - 
---

- What is the name of the BERT geometry paper?
    
    > Visualizing and Measuring the Geometry of BERT
    > 
- Hewitt and Manning Hewitt ([2019](/assets/images/VisualizingandMeasuringtheGeometryofBERThttps://ar5iv.labs.arxiv.org/html/1906.02715#bib.bib7)) BERT seems to create a direct representation of an entire dependency parse tree
    
    The authors find that (after a single global linear transformation, which they term a “structural probe”) the square of the distance between context embeddings is roughly proportional to tree distance in the dependency parse.
    

## Method

- Attention probe method aims to predict a relation based on a *`model-wide attention vector`*
    
    
- *model-wide attention vector is* formed by concatenating the entries for `tokens 1 and 2` in every attention matrix from every attention head in every layer
    - example 12 layer x 12 heads → 144 dimensional vector
- Attention probe can be used to predict whether or not a dependency relation exists or `identity of relation`
    - first is binary classification
    - second is multiclass classification task
    - calculate model wide attention vector for every pair of tokens

## Results

- Attention probe achieved 85.9% accuracy on binary classification and 71.9% on multiclass
    - shows that syntactic information is encoded in attention activations
    

## Analysis

- The BERT geometry author present a mathematical argument for why tree distance is equal to the `square` of the distance in the embedding space
    
    
- We can define a metric space to have a power p embedding if
    
    ![Untitled](/assets/images/VisualizingandMeasuringtheGeometryofBERT/Untitled.png)
    
    - syntactic parse trees appear to have a power 2 embedding in the BERT embedding space
        - based on attention probe
        
- Theorem: any tree with n nodes has a power 2 embedding into $\mathbb{R}^{n-1}$
    - proof by induction
    - Can basically take orthogonal steps in the space to get any distance needed
    
    ![Untitled](/assets/images/VisualizingandMeasuringtheGeometryofBERT/Untitled 1.png)
    
- Clustering in the BERT contextual embedding space
    
    ![Untitled](/assets/images/VisualizingandMeasuringtheGeometryofBERT/Untitled 2.png)
    
- SemCor dataset has sentence pairs using the same keyword in `different senses`
    
    *A: "He thereupon*
    
    *went*
    
    *to London and spent the winter talking to men of wealth."*
    
    *went*
    
    *: to move from one place to another.*
    
    *B: "He*
    
    *went*
    
    *prone on his stomach, the better to pursue his examination."*
    
    *went*
    
    *: to enter into a specified state.*
    
- BERT geometry SemCor experiment individual similarity ratio increases `at higher transformer layers`
    - ratio of average distance to threshold of corresponding and opposite word sense
    - concatenation is from concatenating opposite sense sentences and encoding
    
    ![Untitled](/assets/images/VisualizingandMeasuringtheGeometryofBERT/Untitled 3.png)
    

## Conclusions

This paper presents a simple initial analysis into the geometry of BERT.

## Reference

```python
@article{DBLP:journals/corr/abs-1906-02715,
  author    = {Andy Coenen and
               Emily Reif and
               Ann Yuan and
               Been Kim and
               Adam Pearce and
               Fernanda B. Vi{\'{e}}gas and
               Martin Wattenberg},
  title     = {Visualizing and Measuring the Geometry of {BERT}},
  journal   = {CoRR},
  volume    = {abs/1906.02715},
  year      = {2019},
  url       = {http://arxiv.org/abs/1906.02715},
  eprinttype = {arXiv},
  eprint    = {1906.02715},
  timestamp = {Thu, 13 Jun 2019 13:36:00 +0200},
  biburl    = {https://dblp.org/rec/journals/corr/abs-1906-02715.bib},
  bibsource = {dblp computer science bibliography, https://dblp.org}
}
```