---
title: "XGboost Part 1: Gradient Boosting"
date: 2020-11-02T15:34:30-04:00
categories:
  - 
tags:
---


## Introduction
Xgboost is a powerful yet simple algorithm that has achieved state of the art results on tabular dataset. The Xgboost algorithm uses an ensemble of weak decision tree learners along with gradient boosting. Xboost has several advantages over other gradient boosting approaches. In terms of the algorithm, Xgboost has ways of applying a variety of regularization techniques as well as dealing with sparse inputs. Computationally, Xgboost is highly parallelizable and beats many other algorithms for speed. This article will explain the gradient boosting approach used by XG boost. 

## Gradient Boosting
Boosted tree models work by fitting a number of decision trees in sequence. Each subsequent model seeks to correct the errors made by the previous model. Gradient boosting takes this a step further by using the gradient with respect to the loss function to optimize the model parameters at each step. In many ways, this approach is analagous to gradient descent used in other machine learning algorithms. 

The notation for an ensemble of trees is as follows:


If we have k regression trees, our prediction for y is the sum of the predictions for each tree. We can build intuition for the boosting approach. For k = 1, we start with a weak learner. This base decision tree will make predictions for y. However, this predictions are likely to be underfit, the solution is to fit more learners to predict the difference between the outputs of the first tree and the true values of y. How does this relate to gradient descent?
{% raw %}
$$a^2 + b^2 = c^2$$ 
{% endraw %}






[contact me](mailto:ethan_kim@college.harvard.edu)



