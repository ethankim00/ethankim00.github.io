---
title: "Contrastive Learning for Prompt-Based Few-Shot Language Learners"
last_modified_at: 2022-05-10T20:49:06-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the SupCon paper?
    
    > Contrastive Learning for Prompt-Based Few-Shot Language Learners
    > 
    
    [Yiren Jian](/assets/images/ContrastiveLearningforPrompt-BasedFew-ShotLanguageLearnershttps://arxiv.org/search/cs?searchtype=author&query=Jian%2C+Y)
    , [Chongyang Gao](/assets/images/ContrastiveLearningforPrompt-BasedFew-ShotLanguageLearnershttps://arxiv.org/search/cs?searchtype=author&query=Gao%2C+C)
    , [Soroush Vosoughi](/assets/images/ContrastiveLearningforPrompt-BasedFew-ShotLanguageLearnershttps://arxiv.org/search/cs?searchtype=author&query=Vosoughi%2C+S)
    
- What are the main contributions of the SupCon paper?
    - Supervised Contrastive Learning framework for prompt-based few-shot learners.
    - data augmentation method using
    prompts for contrastive learning with promptbased learners

## Method

- SupCON creates two views of an input text by sampling a `prmompt template and verbalizer`
    
    ![Untitled](/assets/images/ContrastiveLearningforPrompt-BasedFew-ShotLanguageLearners/Untitled.png)
    
- The supcon method basically adds a contrastive loss to the normal MLM loss in PET style classification training
    
    ![Untitled](/assets/images/ContrastiveLearningforPrompt-BasedFew-ShotLanguageLearners/Untitled 1.png)
    

## Results

- SupCON comparison to SIMCLR unclear why they are comparing `supervised to unsupervised method`
    
    ![Untitled](/assets/images/ContrastiveLearningforPrompt-BasedFew-ShotLanguageLearners/Untitled 2.png)
    
- SupCON results with roberta-large base: outperforms other data augmentation methods but is not as good as my differentiable entailment method on `16 shot text classification`
    
    ![Untitled](/assets/images/ContrastiveLearningforPrompt-BasedFew-ShotLanguageLearners/Untitled 3.png)
    

## Conclusions

This paper has the interesting idea to apply contrastive learning with prompt based text view augmentation to help accelerate few shot text classification learning. However it is not competitive with other few shot classification methods like DART and my DE method.

## Reference

```python
@misc{https://doi.org/10.48550/arxiv.2205.01308,
  doi = {10.48550/ARXIV.2205.01308},
  
  url = {https://arxiv.org/abs/2205.01308},
  
  author = {Jian, Yiren and Gao, Chongyang and Vosoughi, Soroush},
  
  keywords = {Computation and Language (cs.CL), Artificial Intelligence (cs.AI), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Contrastive Learning for Prompt-Based Few-Shot Language Learners},
  
  publisher = {arXiv},
  
  year = {2022},
  
  copyright = {arXiv.org perpetual, non-exclusive license}
}
```