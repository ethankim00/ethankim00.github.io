---
title: "LinkBERT: Pretraining Language Models with Document Links"
last_modified_at: 2022-04-06T00:19:30-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the LinkBERT paper?
    
    > LinkBERT: Pretraining Language Models with Document Links
    > 
    
    Michihiro Yasunaga, Jure Leskovec, Percy Liang, Stanford
    
- What are the main contributions of the LinkBERT paper?
    - Document relation prediction task to take advantage of hyperlinks
    - Improvements over baseline LMs on multihop reasoning and multi-document understanding

## Method

- LinkBERT aims to leverage information from `document links`
    
    ![Untitled](/assets/images/LinkBERT:PretrainingLanguageModelswithDocumentLinks/Untitled.png)
    

- How does LinkBERT relate to self-supervised graph learning?
    - DRP → link prediction
    - MLM → node feature prediction
- LinkBERT method  Document Relation Prediction predicts whether inputs are from `contiguous, random or linked documents`
    
    ![Untitled](/assets/images/LinkBERT:PretrainingLanguageModelswithDocumentLinks/Untitled 1.png)
    
- LinkBERT builds off of several avenues of related work
    - retrieval augmented language models: (REALM, RAG)
    - Pretraining LMs with multiple documents: place related documents together
    - Hyperlinks and Citation links
    - Knowledge Graph Augmented Language Models
- LinkBERT experiments with graphs built off of hyperlinks as well as `TFIDF cosine similarity`
    - Hyperlinks are likely to be high precision
- LinkBERT 3 key considerations for retrieving related documents
    - Relevance
    - Salience
        - hyperlinks constructed to provide relevant information
    - Diversity:
        - sample linked document with probability inversely proportional to in-degree

## Results

- LinkBERT is trained off of `wikipedia` and books corpus
    
    
- LinkBERT achieves a small boost on GLUE
    - shows that having related rather than random documents in context enhance learning of language structures
    
    ![Untitled](/assets/images/LinkBERT:PretrainingLanguageModelswithDocumentLinks/Untitled 2.png)
    
- LinkBERT shows good improvement over BERT on `QA benchmarks`
    - intuitively performance should be behind that of retrieval augmented methods
    
    ![Untitled](/assets/images/LinkBERT:PretrainingLanguageModelswithDocumentLinks/Untitled 3.png)
    
- LinkBERT can help to improve `multihop reasoning`
    
    ![Untitled](/assets/images/LinkBERT:PretrainingLanguageModelswithDocumentLinks/Untitled 4.png)
    
- BioMedical LinkBERT is pretrained on `pubmed with citation links`
- BioMed LinkBERT outperforms `pubMED BERT`
    
    ![Untitled](/assets/images/LinkBERT:PretrainingLanguageModelswithDocumentLinks/Untitled 5.png)
    
- What are some benchmarks for biomedical textual understanding?
    - BLURB
    - USMLE: 4 way multiple choice
    - MMLU professional medicine

## Conclusion

This paper presents a simple and effective methods to leverage document hyperlinks in a language model pretraining task. It achieves good performance compared to the base BERT model. It’s mildly interesting that linkBERT performs better  than BERT on GLUE which means linked documents are helping to provide more context for language understanding. One downside is that it’s hard to compare to roBERTa which was trained on much more training data. It seems link unfortunately less data is available with high quality 

## Reference

```json
@article{yasunaga2022linkbert,
  title={LinkBERT: Pretraining Language Models with Document Links},
  author={Yasunaga, Michihiro and Leskovec, Jure and Liang, Percy},
  journal={arXiv preprint arXiv:2203.15827},
  year={2022}
}
```