---
title: "TransformerXL"
date: 2022-02-27T21:17:21-05:00
categories: paper_review
tags:
	- Machine Learning
	- NLP
	- Transformer
---

- What is the name of the Transformer XL paper?
    
    > Transformer-XL: Attentive Language Models
    Beyond a Fixed-Length Context
    > 
    
    Zihang Dai, CMU/ Google Brain 
    
- The major achievement of transformerXL is the `ability to capture more long range dependencies`
    - Longer context than RNNs or vanilla transformers
    - Relative rather than absolute positional embedding
    
- A problem with Vanilla transformers is the fixed `context size`
    - doesn't necessarily respect sentence boundaries
    - Have to compute over again at each step
    
    ![/Untitled.png](/assets/images/TransformerXL/Untitled.png)
    
- TransformerXL seeks to extend the context window of transformers by introducing a `recurrence mechanism`
    - Cache hidden state representation of previous context window
    
    ![/Untitled%201.png](/assets/images/TransformerXL/Untitled%201.png)
    
    ![/Untitled%202.png](/assets/images/TransformerXL/Untitled%202.png)
    
- TransformerXL `concatenates` the previous context windows hidden state
    - Do not update the gradients for the previous representation
    
    ![/Untitled%203.png](/assets/images/TransformerXL/Untitled%203.png)
    
- The amount of context encoded by transformersXL is dependent on the `number of transformer layers`
    - Context from lower transformer layers will propagate further
    
    ![/Untitled%204.png](/assets/images/TransformerXL/Untitled%204.png)
    
- TransformerXL incorporates more previous context using `relative positional embeddings`
    - Sinusoidal with no Learned Parameters
- What is the key idea behind relative positional embeddings?
    - A query doesn't need to know the absolute position of a key, only the position relative to itself
    
- How can the attention score in transformers be factored out to see the impact of embeddings and content vectors?
    
    FOIL
    
    ![/Untitled%205.png](/assets/images/TransformerXL/Untitled%205.png)
    
- The relative positional embedding in transformersXL replaces absolute positional embeddings for the `key vectors`
    
    R is the sinusoidal relative positional embedding
    
    ![/Untitled%206.png](/assets/images/TransformerXL/Untitled%206.png)
    
    - Also replaces the positional embedding for the query with a learned constant
        - Intuition is that no information is lost since we have a relative embedding between query and key
    - Finally separates out positional and content parts of the key generation
- TransformerXL relative positional embeddings replace the positional embeddings for query vectors because `information is already encoded in relative key vector`
    
    ![/Untitled%207.png](/assets/images/TransformerXL/Untitled%207.png)
    
- TransformerXL achieves SOTA `perplexity` results on many datasets
    
    ![/Untitled%208.png](/assets/images/TransformerXL/Untitled%208.png)