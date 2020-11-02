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
{% endraw %}

If we have k regression trees, our prediction for y is the sum of the predictions for each tree. We can build intuition for the boosting approach. For k = 1, we start with a weak learner. This base decision tree will make predictions for y. However, this predictions are likely to be underfit, the solution is to fit more learners to predict the difference between the outputs of the first tree and the true values of y. How does this relate to gradient descent? Lets assume that we have a regression problem with squared error loss. 

{% raw %}
$$l(\hat(y), y) = \sum_i^n (\hat{y}_i - y_i)^2$$
{% endraw %}
{% raw %}
$$
\frac{\partial l}{\partial y}l(\hat{y}, y) = 2\sum_i^n (\hat{y}_i - y_i)
$$
{% endraw %}



We can see that the derivative is proportional to the resiudals between the actual and predicted values. This result provides the intuition for fiting boosted models on the residuals from a models output. The additive outputs of each subsequent model minimize the loss by stepping along the gradient. For the case of tree ensembles subsequent trees assign each observation to a leaf node with a weight that represents the optimal "tweak" to the predictions of the previous model.

Gradient boosting uses a computation of the gradient of the loss function to determine what weights to use. For Xgboost, the loss function includes a regularization term:
{% raw %}
$$
L(\phi) = \sum_il(\hat{y}_i, y) + \sum_k \Omega (f_k)
$$
{% endraw %}
{% raw %}
$$
\Omega (f) = \gamma T + \frac{1}{2} \lambda||w||^2
$$
{% endraw %}
Here T is the number of leaf nodes. In this example, we are using l1 regression

In the xgboost algorithm we sequentially fit a series of learners. At a specific step, the loss will depend on the previous models output as well as the current models updates:
{% raw %}
$$
L^{(t)} = \sum_i^n l(y_u, \hat{y}_i^{(t-1)} + f_t(x_i)) + \Omega (f_t)
$$
{% endraw %}

This loss can be approximated using a second order Taylor series? 

Why do we use this Taylor series approximation for the loss? The purpose is to represent the loss as in expression in terms of the parameters of the tree nodes that can be optimized. 


[contact me](mailto:ethan_kim@college.harvard.edu)



