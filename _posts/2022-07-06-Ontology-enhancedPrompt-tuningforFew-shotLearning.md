---
title: "Ontology-enhanced Prompt-tuning for Few-shot Learning"
last_modified_at: 2022-07-06T21:53:33-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What is the name of the OntoPrompt paper?
    
    > Ontology-enhanced Prompt-tuning for Few-shot Learning
    > 
    
- What are the main contributions of the OntoPrompt paper?
    - ontology tranformation  process to convert structured knowledge to text
    - span sensitive knowledge injection  inject external knowledge but avoid injecting noise
    - collective training jointly train representation: inject ontology tokens
- OntoPrompt works for three tasks: `relation extraction, event extraction and knowledge graph completion`
    
    
- What are some challenges with injecting knowledge for few shot learning?
    - **missing knowledge:** external knowledge base may be missing information
    - **knowledge noise:** knowledge might not be beneficial for downstream tasks
    - **knowledge heterogeneity:** difference in language between downstream tasks and injected knowledge source
    
- OntoZSL incorporate ontology prior knowledge for `class semantics`
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled.png)
    

## Method

- In ontoprompt auxiliary knowledge is `converted into text` and `append them to input sequence`
    
    
- In OntoPrompt how is the reference ontology formulated?
    - $\mathbb{O}= \{\mathcal{C}, \mathcal{E}, \mathcal{D}\}$
    - C is set of concepts
    - E are connected edges
    - D textual description of each ontology
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled 1.png)
    
- OntoPrompt is applied to relation extraction
    - named entity descriptions from MUC
    - use external textual descriptions in prompt as well as virtual tokens
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled 2.png)
    
- OntoPrompt for event extraction: prompt has MASK tokens for `Event trigger, event type and each event argument`
    - each one is enhanced with ontology descriptive text
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled 3.png)
    
- OntoPrompt for knowledge graph completion is framed as a `triple classification task`
    - get ontology descriptions from wikidata
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled 4.png)
    
- OntoPrompt span sensitive knowledge injection is done with a `custom attention mask`
    - input can attend to itself
    - tokens within each ontology and template can attend to themselves
    - span for mentioned entities can attend to **their own entity descriptions**
    - template tokens attend to everything
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled 5.png)
    
- OntoPrompt collective training `optimizes ontology tokens` then finetunes entire model
    - ontology tokens are initialized from
    - similar to two stage training in DART (same author)

## Results

- OntoPrompt ablation: span sensitive knowledge injection leads to `small boost`
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled 6.png)
    
- OntoPrompt RE results: beats `GDPNET`
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled 7.png)
    
- OntoPrompt case study: interpretability by looking at `nearest neighbors of virtual tokens`
    
    ![Untitled](/assets/images/Ontology-enhancedPrompt-tuningforFew-shotLearning/Untitled 8.png)
    

## Conclusions

This paper extends the training of virtual tokens in DART to enhance few shot performance on new types of NLP tasks. The injection of text from an external knowledge ontology is very similar to the metadata shaping and TaBi methods. 