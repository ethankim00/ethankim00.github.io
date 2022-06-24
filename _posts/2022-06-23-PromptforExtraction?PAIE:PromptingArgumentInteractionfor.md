---
title: "Prompt for Extraction? PAIE: Prompting Argument Interaction for"
last_modified_at: 2022-06-23T23:08:26-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the PAIE paper?
    
    > Prompt for Extraction? PAIE: Prompting Argument Interaction for
    Event Argument Extraction
    > 
    
- What are the main contributions of the PAIE paper?
    - prompt tuning method for EAE
- Two main approaches for EAE include semantic role labeling and `question answering/text generation`
    - Semantic Role Labeling: Identify spans + classify their roles
    - 
- Semantic Role labeling approaches for EAE have two basic steps: `identifying candidate spans and classifying their roles`
    
    
- One disadvantage of QA based models for EAE is the need to `frame a separate question for each argument`
    - ex: NLI+ paper need to use multiple templates and forward passes through NLI model
    - Generative methods can generate all at once:
        - however current performance of these types of methods are not great
- What are the main goals of the PAIE paper?
    - extract all arguments efficiently
    - how to capture argument interactions in long text on the fly
    - how can we leverage knowledge from
- EAE tasks can be framed on both the sentence and `document level`
    - some may require identifying antecedents of different arguments
    
    ![Untitled](/assets/images/PromptforExtraction?PAIE:PromptingArgumentInteractionfor/Untitled.png)
    
- QA-based methods usually adopt the thresholding strategy which must be `manually tuned`
    - likelihood threshold for QA argument extraction answer

## Method

- The three main steps of the PAIE method are 1) prompt creation, 2) span selector decoding and 3) `span prediction`
    - prompt is generated from a series of queries
    - prompted selector decoding: spans selected based on `input`, `prompt` and `query`
    - prompted span selection: selectors for start and end of optimal span
- PAIE feeds span and context into `BART backbone`
    - include special markers before and after trigger tokens
    
    ![Untitled](/assets/images/PromptforExtraction?PAIE:PromptingArgumentInteractionfor/Untitled 1.png)
    
- PAIE span selector head uses `start and end token hidden representation as features`
    - can simultaneously score all roles by combing all k queries in one matrix multiply by the meta prediction head
    
    ![Untitled](/assets/images/PromptforExtraction?PAIE:PromptingArgumentInteractionfor/Untitled 2.png)
    

## Results

- PAIE main results on ACE and Wikievents
    
    ![Untitled](/assets/images/PromptforExtraction?PAIE:PromptingArgumentInteractionfor/Untitled 3.png)
    

## Conclusions

- Does worse than NLI+ paper on wikievents but seemingly better on ACE

## Reference

```json
@misc{https://doi.org/10.48550/arxiv.2202.12109,
  doi = {10.48550/ARXIV.2202.12109},
  
  url = {https://arxiv.org/abs/2202.12109},
  
  author = {Ma, Yubo and Wang, Zehao and Cao, Yixin and Li, Mukai and Chen, Meiqi and Wang, Kun and Shao, Jing},
  
  keywords = {Computation and Language (cs.CL), Artificial Intelligence (cs.AI), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Prompt for Extraction? PAIE: Prompting Argument Interaction for Event Argument Extraction},
  
  publisher = {arXiv},
  
  year = {2022},
  
  copyright = {Creative Commons Attribution 4.0 International}
}
```