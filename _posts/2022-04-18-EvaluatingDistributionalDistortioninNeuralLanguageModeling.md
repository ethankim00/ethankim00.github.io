---
title: "Evaluating Distributional Distortion in Neural Language Modeling"
last_modified_at: 2022-04-18T21:17:27-05:00
categories: paper_review
tags:
    - Machine Learning
---


## Introduction

    
    > ****Evaluating Distributional Distortion in Neural Language Modeling****
    > 
    
    Benjamin LeBrun, Alessandro Sordoni, Timothy J. O’Donnell
    
- What are the main contributions of the Distributional Distortion paper?
    
    Evaluate LM performance on Heavy tail of sequence distributions
    
    - instance level evaluation scheme
    - Use a transformer generative model to define artificial languages
        - can control distributional properties
    - Language Model Performance
        - Systematically underestimate probabilities of rare sequences
        - Do so more for rare sequences
        - overestimate probability of rare (ill-formed) sequences
        - Greater amounts of training data alleviate underestimation
        - underestimation is worse for distributions with lower entropy
- Productivity in Natural Language refers to the phenomenon that `a speaker can produce an infinite number of gramatically valid utterances`
    - assigned nonzero probability
- LNRE zone refers to the `range of sample sizes which have nonzero probability of generating an unseen sequence`
    - LNRE holds for language models and is expected to hold for many more orders of magnitude of sample data
- The Kleene closure is the `set of all strings of finite length`

## Method

- The Distributional Distortion paper: generates artificial languages with `GPT2-medium`
    - sample from this language model’s distribution
- Distributional Distortion measures model estimation error using the `difference in logs of probabilities of sequence under true process and a trained language model process`
    
    

## Results

- Language model estimation error is minimized when `model reaches lowest cross entropy`
    - note how GPT2 is able to perfectly replicate the training distribution after 5 epochs (left)
    
    ![Untitled](/assets/images/EvaluatingDistributionalDistortioninNeuralLanguageModeling/Untitled.png)
    
- Distributional Distortion results low probability sequences are `consistently underestimated`
    - real rare event are underestimated in favor of perturbed rare events
    
    ![Untitled](/assets/images/EvaluatingDistributionalDistortioninNeuralLanguageModeling/Untitled 1.png)
    
- Distributional Distortion sequences with higher temperature (higher entropy) have `less estimation error`
    - language models better model sequences with less heavy tails

## Reference

```python
@article{https://doi.org/10.48550/arxiv.2203.12788,
  doi = {10.48550/ARXIV.2203.12788},
  
  url = {https://arxiv.org/abs/2203.12788},
  
  author = {LeBrun, Benjamin and Sordoni, Alessandro and O'Donnell, Timothy J.},
  
  keywords = {Computation and Language (cs.CL), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Evaluating Distributional Distortion in Neural Language Modeling},
  
  publisher = {arXiv},
  
  year = {2022},
  
  copyright = {arXiv.org perpetual, non-exclusive license}
}
```