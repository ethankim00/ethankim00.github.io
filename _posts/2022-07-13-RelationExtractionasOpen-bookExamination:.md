---
title: "Relation Extraction as Open-book Examination:"
last_modified_at: 2022-07-13T08:40:17-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction




- What is the name of the retrievalRE paper?
    
    > Relation Extraction as Open-book Examination:
    Retrieval-enhanced Prompt Tuning
    > 
    
- What are the main contributions of the retrievalRE paper?
    - retrieval enhanced prompt tuning for relation extraction
    - augment RE with an openbook datastore
    - allow for memorization of key value pairs
    - SOTA on full and few shot RE tasks
- KnowPrompt introduced learnable `virtual type words`
    - inject semantic knowledge
    
    ![Untitled](/assets/images/RelationExtractionasOpen-bookExamination:/Untitled.png)
    
- Knowprompt Entity knowledge injection inject trainable id tokens for `entity types`
    - ex: person, org
    
    ![Untitled](/assets/images/RelationExtractionasOpen-bookExamination:/Untitled 1.png)
    
- Knowprompt virtual type words are similar to `DART differential tuning of verbalizer`

## Method

- The main idea behind retrieval RE is to allow model to `refer to similar instances at inference time`
    
    
- RetrievalRE inference procedure: final prediction distribution over relations is the `interpolation between model MLM head and the kNN retrieved distribution`
    
    ![Untitled](/assets/images/RelationExtractionasOpen-bookExamination:/Untitled 2.png)
    

## Results

- RetrievalRE few shot results: takes advantage of `memorizing training examples`
    
    ![Untitled](/assets/images/RelationExtractionasOpen-bookExamination:/Untitled 3.png)
    
- Example retrievalRE retrieval procedure correcting logits from `pure prompt tuned model`
    
    ![Untitled](/assets/images/RelationExtractionasOpen-bookExamination:/Untitled 4.png)
    
- One advantage of the key-value datastore for RetrievalRE is that is allows for `post hoc model editing`
    - similar to TaBi typed embeddings
    
- RetrievalRE ablation: retrieval representation `doesn't really matter`
    - simple tfidf method performs almost as well
    
    ![Untitled](/assets/images/RelationExtractionasOpen-bookExamination:/Untitled 5.png)
    
- PET

## Conclusions

Retrieval method depends very strongly on train and test overlap. It feels like cheating if there is a very similar instance in the training set and we can simply look up its relation. The approach is very similar to the KATE paper which selects nearest neighbor examples for GPT3 in context prompts. 

## Reference

```python
@article{Chen2022RelationEA,
  title={Relation Extraction as Open-book Examination: Retrieval-enhanced Prompt Tuning},
  author={Xiang Chen and Lei Li and Ningyu Zhang and Chuanqi Tan and Fei Huang and Luo Si and Huajun Chen},
  journal={ArXiv},
  year={2022},
  volume={abs/2205.02355}
}
```