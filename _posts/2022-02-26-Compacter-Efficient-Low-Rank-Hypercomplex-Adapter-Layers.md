---
title: "Compacter"
last_modified_at: 2022-02-26T16:20:02-05:00
categories: paper_review
tags: 
    - "parameter_efficiency"
    - "adapters"
---

# Compacter

- What is the name of the compacter paper?
    
    > COMPACTER:
    Efficient Low-Rank Hypercomplex Adapter Layers
    > 
    
- Compacter paper was written by `Sebastian Ruder`
    
    
- With four parameters I can fit an elephant,
and with five I can make him wiggle his trunk.
    
    John von Neumann
    
    Warning against overparameterized models
    
- Compacters efficient version of compacter layers are called `parameterized hypercomplex multiplication layers`
    
    
- Kronecker product can be considered a generalization of an `outer product`
    - outer product of two vectors is a rank 1 matrix
    - Kronecker product of two matrices is a large matrix
    
    ![Untitled](Compacter%20a9a7d/Untitled.png)
    
- Kronecker product is also known as the `matrix direct` product
    - is bilinear and associative
    
- Adapters contain a `down projection` as well as an up projection
    - bottlenect
    - opposite of Transformer FFN
    
    ![Untitled](Compacter%20a9a7d/Untitled%201.png)
    
- In a parameterized hypercomplex multiplication layer the weight matrix is a sum of `kronecker products`
    
    $W = \sum_{i= 0}^n A_i \otimes B_i$
    
    Parameter complexity is $O(kd/n)$
    
    reduces parameters by factor  $\frac{1}{n}$
    
    ---
    
- In Compacter the adapter weights are a product of `shared task specific parameters and adapter layer specific parameters`
    - Factor both up and down projection weights
    
    ![Untitled](Compacter%20a9a7d/Untitled%202.png)
    
- What two techniques does the compacter paper introduce to make hypercomplex adapters more efficient?
    - shared information across adapters
    - low rank parameterization
    - Reduces complexity to O(k + d)
    
- Low rank parameterization is achieved in hypercomplex adapters by `restricting B to be of rank 1`
    
    ![Untitled](Compacter%20a9a7d/Untitled%203.png)
    
- Compacter base model is `T5 base`
    
    220m parameters 
    
- Parameter efficient adapter baselines
    - Pffeifer adapter: only one adapter in each layer
    - Adapter Low Rank
    - Prompt tunining (Lester)
    - Intrinsic SAID
    - Adapter Drop
        - drop adapters from lower transformer layers
    - Bitfit freeze weights + train only biases
    
- The idea behind bitfit is to `freeze model layers and train only biases`