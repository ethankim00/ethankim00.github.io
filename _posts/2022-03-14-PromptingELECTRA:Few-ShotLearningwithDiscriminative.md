---
title: "Prompting ELECTRA: Few-Shot Learning with Discriminative"
last_modified_at: 2022-03-14T20:26:38-05:00
categories: paper_review
tags:
    - Few Shot Learning
    - Machine Learning
    - Parameter Efficiency
---

## Introduction

This new paper on few shot prompt based earning leverages ELECTRA, a special type of language model that is trained on 

- What is the name of the prompting Electra for Few Shot Learning paper?
    
    > ****Prompting ELECTRA: Few-Shot Learning with Discriminative Pre-Trained Models****
    > 
    
- What are the main contributions of the prompting Electra for Few Shot Learning paper?
    - method to use ELECTRA in 0 and few shot settings
    - outperforms BERT and roBERTa on 13 tasks
    
- Background ELECTRA model is trained to predict whether a token was `corrupted by a MLM`
    - MLM and discriminator are trained jointly
    - note that only the discriminator is kept and applied to downstream tasks
    
    ![Untitled](/assets/images/PromptingELECTRA:Few-ShotLearningwithDiscriminative/Untitled.png)
    
- Cloze style MLMs deal with multitoken verbalizers use a `heuristic decoding method`
    - kind of a hacky workaround
    - this solution would not be necessary with expanded vocabulary models like [WordBERT](https://ethankim00.github.io./paper_review/PretrainingwithoutWordpieces-LearningOveraVocabularyofMillionsofWords/)


## Method

- How can we prompt ELECTRA for single token verbalizers
    - Define a template “{sentence} it was {label}
    - Do forward passes for each distinct label
        - Very similar to entailment as few shot learner
    - Prediction is a label that has the highest probability of being an `original token`
        - remember that this matches the ELECTRA pretraining objective
    
    ![Untitled](/assets/images/PromptingELECTRA:Few-ShotLearningwithDiscriminative/Untitled 1.png)
    
- How do we adapt to prompt ELECTRA for multitoken verbalizers?
    
    There are two approaches:
    
    - average the hidden representation of all tokens and calculate the probability using the classification head on top of this mean pooled representation
        - they call this method “rep”
    - use average probability of each token (average verbalized output probability)
        - they all this method “prob”
    - It’s pretty intuitive that the “rep” method would work better and indeed that’s what they found

## Results

- Prompting ELECTRA results: better performance in the `few shot k = 16` setting
    - zero shot performance is also impressive (includes prompt)
    
    ![Untitled](/assets/images/PromptingELECTRA:Few-ShotLearningwithDiscriminative/Untitled 2.png)
    
    - note that I get better results in my Differentiable Entailment paper

- ELECTRA few shot prompting results on multiple choice tasks `outperforms` ROBERTA PET
    - note that ELECTRA requires multiple forward passes effectively scaling the inference cost linearly with the number of choices
    
    ![Untitled](/assets/images/PromptingELECTRA:Few-ShotLearningwithDiscriminative/Untitled 3.png)
    
- ELECTRA few shot prompting performance scales with `number of examples`
    - note gap narrows with other few shot methods
    
    ![Untitled](/assets/images/PromptingELECTRA:Few-ShotLearningwithDiscriminative/Untitled 4.png)
    

## Conclusions

This paper proposes an interesting approach to using discriminative models as classifiers and applying prompting in the few-shot learning regime. Note that they still use some manual prompting instead of fully training prompts without any domain knowledge as is done in other methods. The model outperforms baselines showing the validity of the approach. However it’s important to note that other few-shot learning approaches leveraging BERT/roBERTa do have stronger performance (including my upcoming method which is also more parameter efficient). Overall it’s nice to see more basic research into discriminative models given that roBERTa is dominant for many practical use cases. 

## Reference

```python
https://openreview.net/forum?id=SdOLXkjaq19
```