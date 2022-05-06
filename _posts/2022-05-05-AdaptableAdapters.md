---
title: "Adaptable Adapters"
last_modified_at: 2022-05-05T22:25:59-05:00
categories: paper_review
tags:
    - Machine Learning
---

## Introduction

- What are the main contributions of the adaptable adapters paper?
    - Adaptable Adapters:
        1. learning activation functions for different inputs and layers
        2. learnable switch to use only beneficial adapter layers
    - Adapters require less training time and storage space
    - using fewer adapters with a learnable activation increases performance in the low data domain
- Padé Activation Units (Molina et al.,
2020), are `learnable activation functions` that can
approximate common activation functions as well
as learn new ones.
- Rational activation units are parameterized by `two` parameters
    - order m, n
    - a and b are learnable parameters
    
    ![Untitled](/assets/images/AdaptableAdapters/Untitled.png)
    
- Schwartz et al. (2020) propose to add an `output
layer to` each transformer layer.
    - confidence based early exiting for efficiency
- AdapterDrop (Rücklé et al., 2021) train adapters with layer dropping and later `drop n layers at inference time`

## Method

- Adaptable Adapter layer uses a `rational` activation function
    
    ![Untitled](/assets/images/AdaptableAdapters/Untitled 1.png)
    
- Adaptable adapters uses a `gumbel softmax` to learn when to skip the layer
    - skipping applied skip connection
    
    ![Untitled](/assets/images/AdaptableAdapters/Untitled 2.png)
    

## Results

- Adaptive Adapter results: better performance in the `low data regime`
    - adapters are on top of BERT large
    
    ![Untitled](/assets/images/AdaptableAdapters/Untitled 3.png)
    
- AA learned rational optimization functions
    
    ![Untitled](/assets/images/AdaptableAdapters/Untitled 4.png)
    

## Reference

```jsx
 @inproceedings{Moosavi2022AdaptableA,
  title={Adaptable Adapters},
  author={Nafise Sadat Moosavi and Quentin Delfosse and Kristian Kersting and Iryna Gurevych},
  year={2022}
}
```