---
title: "Neural reality of argument structure constructions"
last_modified_at: 2022-03-13T11:36:04-05:00
categories: paper_review
tags:
    - Paper
---
## Introduction

This paper aims to analyze transformer models from a psycholinguistic viewpoint

- The major contributions of the Neural Reality of Argument Structure Constructions paper are
    - Transformer encode sentences with same ASC as more similar than sentences with the same verb
        - This is an emergent property of transformers at scale
    - Transformers derive meaning from Argument Structure Constructions without presence of lexical cues
- Argument structure constructions refers to pairs in linguistics that create specific meanings
    - proposed by construction grammarians
    
    ![Untitled](/assets/images/Neuralrealityofargumentstructureconstructions/Untitled.png)
    

- Argument Structure Constructions contrast with lexicalist theories which hold that `structure is encoded in the verb`
    - For a lexicalist the word visit is known to be transitive
    - ASC has better empirical supported
    
- Evidence for human use of Argument Structure Constructions comes from the `index card sorting study`
    - Humans sorted index cards more by construction than by verb
    - Repeated in other languages
    - More advanced foreign language learners were more likely to prefer construction
    
- Human priming effect “Jabberwocky” priming increased response performance on `lexical decision task`
    - Prime with nonsense “He daxed her the narp”
    - Quicker to identify real verbs which shared that construction eg “gave”, “handed”
    
- Previous work on linguistic probing of LMs
    - LM can be evaluated via template generation
    - BERT classification task on whether two sentences share the same argument
- Previous psycholinguistic treatment of LMs
    - BERT often fails to understand negation
    - Can look at LM surprisals due to linguistic anomalies
    - Often based on psycholinguistic datasets that suffer from small size

## Method

- Sentence sorting experiment for neurolinguistic probing of transformers
    - Sample 4 verbs and 4 constructions
    - Generate embeddings based on construction template, fill in nouns and proper nouns
        - note: they mean pool the second to last transformer layer
    - Group into 4 clusters using HAC by euclidean distance
    - Calculate deviation from pure verb or pure construction sort using the Hungarian algorithm
    
- “Jabberwocky” task probing language models place random verb in construction and measure distance to `prototype verb for that construction`
    - again evaluation is based on contextual embeddings of sentences
    - embed the verb using average contextual embedding across 4m word corpus
    
    ![Untitled](/assets/images/Neuralrealityofargumentstructureconstructions/Untitled 1.png)
    

## Results

- Psycholinguistic sentence sorting experiment results: MiniroBERTa trained on more data had `more constructionist bias`
    
    ![Untitled](/assets/images/Neuralrealityofargumentstructureconstructions/Untitled 2.png)
    

- Jabberwocky experiment results: euclidean distance is smaller to the `prototype verb with the matching construction`
    - small yet significant margin
    - shows how transformers encode the ASC into the contextual verb representaion
    
    ![Untitled](/assets/images/Neuralrealityofargumentstructureconstructions/Untitled 3.png)
    

## Conclusions

Evaluating language models based on linguistic theories provides an interesting perspective. I do think that the choice of evaluation methods could have been better. For the jabberwocky task evaluating based on the distance to prototype verbs is complicated. A simpler and more interpretable method would be to train a linear classifier as a probe to predict the ASC category from the verb’s contextual representation. Any improvement in accuracy over random guessing would indicate that the transformer is encoding ASCs. Similarly, for the sentence sorting task, we could train a classifier to predict the verb and predict the ASC and compare performance. I think these methods are much more intuitive than the complicated step of doing HAC followed by the Hungarian algorithm. This would ameliorate the instabilities of taking an arbitrary number of clusters from HAC. Further follow-up research could show how the understanding of ASCs transfers to downstream tasks or how pretraining tasks can be constructed to promote transformers to understand ASCs with limited training data.

## Reference

```python
@article{li2022neural,
title={Neural reality of argument structure constructions },
author={Bai Li and Zining Zhu and Guillaume Thomas and Frank Rudzicz and Yang Xu },
year={2022},
journal={arXiv preprint arXiv: Arxiv-2202.12246}
}
```