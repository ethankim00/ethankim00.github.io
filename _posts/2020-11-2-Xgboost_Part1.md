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

{% raw %}
$$\hat{y}_i = \phi(x_i) = \sum_{k=1}^K f_k(x_i), f_k \in F$$
{% raw %}

If we have k regression trees, our prediction for y is the sum of the predictions for each tree. We can build intuition for the boosting approach. For k = 1, we start with a weak learner. This base decision tree will make predictions for y. However, this predictions are likely to be underfit, the solution is to fit more learners to predict the difference between the outputs of the first tree and the true values of y. How does this relate to gradient descent? Lets assume that we have a regression problem with squared error loss. 

{% raw %}
$$l(\hat(y), y) = \sum_i^n (\hat{y}_i - y_i)^2$$
$$
\frac{\partial l}{\partial y}l(\hat{y}, y) = 2\sum_i^n (\hat{y}_i - y_i)
$$
{% raw %}

We can see that the derivative is proportional to the resiudals between the actual and predicted values. This result provides the intuition for fiting boosted models on the residuals from a models output. The additive outputs of each subsequent model minimize the loss by stepping along the gradient. For the case of tree ensembles subsequent trees assign each observation to a leaf node with a weight that represents the 

Gradient boosting uses a computation of the gradient to 



[contact me](mailto:ethan_kim@college.harvard.edu)



