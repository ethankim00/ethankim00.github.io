---
title: "On Transferability of Prompt Tuning for Natural Language Processing"
last_modified_at: 2022-10-11T21:28:05-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the THUNLP prompt transfer paper?
    
    > On Transferability of Prompt Tuning for Natural Language Processing
    > 
    
- What are the main contributions of the THUNLP prompt transfer paper?
    - trained soft prompts can transfer zero shot to other tasks
    - can transfer across models with a cross model projector
    - as initialization soft prompts can significantly accelerate training
    - explore the effect of various prompt similarity metrics as transferability indicators
- One issue with prompt tuning is it is sometimes `slower to converge` compared to full finetuning
    
    ![Untitled](/assets/images/OnTransferabilityofPromptTuningforNaturalLanguageProcessing/Untitled.png)
    
- roberta-large: zero shot transfer performance: best transfer prompt `varies widely`
    - do they use the same roberta classification head or a PET style verbalizer?
    
    ![Untitled](/assets/images/OnTransferabilityofPromptTuningforNaturalLanguageProcessing/Untitled 1.png)
    

## Method

- Cross task and cross model soft prompt transfer
    
    ![Untitled](/assets/images/OnTransferabilityofPromptTuningforNaturalLanguageProcessing/Untitled 2.png)
    
- How can we transfer soft prompts across model architectures?
    - use a learnable projector (2 layer perceptron)
- What objective can we use to train a cross model soft prompt projector?
    - distance minimization: minimize distance between prompt trained on the same task
    - task specific: backpropagate weights to the source prompt
        - hopefully can generalize to transfer prompts for other tasks

## Results

- THUNLP soft prompt transfer results: initialization transfer leads to `marginal boost in performance`
    - convergence speedup is quite high for T5
    
    ![Untitled](/assets/images/OnTransferabilityofPromptTuningforNaturalLanguageProcessing/Untitled 3.png)
    
- Cross Model prompt transfer results:
    - doesn’t seem to work particulary well
    
    ![Untitled](/assets/images/OnTransferabilityofPromptTuningforNaturalLanguageProcessing/Untitled 4.png)
    

## Conclusions

Similar in idea to the SPOT paper. I didn’t find the transferability indicators that interesting since a similar prompt is always likely to give the best transfer task and also the overlapping neuron activation rate was rather poorly motivated.

## Reference

```python
@article{su2021transferability,
  title={On transferability of prompt tuning for natural language understanding},
  author={Su, Yusheng and Wang, Xiaozhi and Qin, Yujia and Chan, Chi-Min and Lin, Yankai and Liu, Zhiyuan and Li, Peng and Li, Juanzi and Hou, Lei and Sun, Maosong and others},
  journal={arXiv preprint arXiv:2111.06719},
  year={2021}
}
```