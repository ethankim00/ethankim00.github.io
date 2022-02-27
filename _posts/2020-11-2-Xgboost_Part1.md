---
title: "XGboost Part 1: Gradient Boosting"
date: 2020-11-02T15:34:30-04:00
categories:
  - method
tags:
  - xgboost
---

## Introduction
Xgboost is a powerful yet simple algorithm that has achieved state of the art results on tabular datasets. The Xgboost algorithm uses an ensemble of weak decision tree learners along with gradient boosting. Xboost has several advantages over other gradient boosting approaches. In terms of the algorithm, Xgboost has ways of applying a variety of regularization techniques as well as dealing with sparse inputs. Computationally, Xgboost is highly parallelizable and beats many other algorithms for speed. This article will explain the gradient boosting approach used by XG boost. 

## Gradient Boosting
Boosted tree models work by fitting a number of decision trees in sequence. Each subsequent model seeks to correct the errors made by the previous model. Gradient boosting takes this a step further by using the gradient with respect to the loss function to optimize the model parameters at each step. In many ways, this approach is analagous to gradient descent used in other machine learning algorithms. 

The notation for an ensemble of trees is as follows:

{% raw %}
$$\hat{y}_i = \phi(x_i) = \sum_{k=1}^K f_k(x_i), f_k \in F$$
{% endraw %}

If we have k regression trees, our prediction for y is the sum of the predictions for each tree. We can build intuition for the boosting approach. For k = 1, we start with a weak learner. This base decision tree will make predictions for y. However, this predictions are likely to be underfit. The solution is to fit more learners to predict the difference between the outputs of the first tree and the true values of y. How does this relate to gradient descent? Lets assume that we have a regression problem with squared error loss. 

{% raw %}
$$l(\hat(y), y) = \sum_i^n (\hat{y}_i - y_i)^2$$
{% endraw %}
{% raw %}
$$
\frac{\partial l}{\partial y}l(\hat{y}, y) = 2\sum_i^n (\hat{y}_i - y_i)
$$
{% endraw %}



We can see that the derivative is proportional to the residuals between the actual and predicted values. This result provides the intuition for fiting boosted models on the residuals from a models output. The additive outputs of each subsequent model minimize the loss by stepping along the gradient. For the case of tree ensembles, subsequent trees assign each observation to a leaf node with a weight that represents the optimal "tweak" to the predictions of the previous model.

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
Here T is the number of leaf nodes. In this example, we are using a l2 penalty.

In the xgboost algorithm we sequentially fit a series of learners. At a specific step, the loss will depend on the previous models output as well as the current models updates:
{% raw %}
$$
L^{(t)} = \sum_i^n l(y_u, \hat{y}_i^{(t-1)} + f_t(x_i)) + \Omega (f_t)
$$
{% endraw %}

This loss can be approximated using a second order Taylor series:
{% raw %}
$$
L^{(t)} \approx \sum_{i =1}^n [l(y_i, \hat{y}^{(t-1)}) +g_if_t(x_i) + \frac{1}{2}h_if_t^2(x_i)] + \Omega(f_t)
$$
{% endraw %}
{% raw %}
Here $g_i = \partial_{\hat{y}^{(t-1)}}l(y_i, \hat{y}^{(t-1)})$ and $h_i = \partial_{\hat{y}^{(t-1)}}^2l(y_i, \hat{y}^{(t-1)})$
{% endraw %}

Why do we use this Taylor series approximation for the loss? The purpose is to represent the loss as an expression in terms of the parameters of the tree nodes that can be optimized. 

To continue, we can remove the constant terms that do not depend on f.
{% raw %}
$$
\hat{L}^{(t)} = \sum_{i=1}^n [g_i f_t(x_i) + \frac{1}{2}h_i f_t^2 (x_i)] + \Omega(f_t)
$$
{% endraw %}

{% raw %}
We can define $ I_j = (i|q(x_i))$ to be the set of observations that fall within leaf j of a tree
{% endraw %}

{% raw %}
$$
\tilde{L}^{(t)} = \sum_{i =1}^n [g_if_t(x_i) + \frac{1}{2}h_if_t^2(x_i)] + \gamma T + \frac{1}{2}\lambda \sum_{j=1}^2w_j^2
$$
{% endraw %}

{% raw %}
$$
= \sum_{j=1}^T[(\sum_{i \in Ij} g_i)w_j + \frac{1}{2}(\sum_{i \in I_j} h_i + \lambda) w_j^2] + \gamma T
$$
{% endraw %}
{% raw %}
We can rewrite the loss function approximation as a sum over all leaf nodes. The predictions, $f_t(x_i)$ are equal to the weight $w_j$ for the node in which observation $x_i$ falls. To find the optimal $w_j$, use calculus:
{% endraw %}

Within a single leaf node j, we can calculate the derivative of the loss with respect to the weight:

{% raw %}
$$
\frac{\partial}{\partial w}[(\sum_{i \in Ij} g_i)w_j + \frac{1}{2}(\sum_{i \in I_j} h_i + \lambda) w_j^2] + \gamma T
$$
{% endraw %}
{% raw %}
$$
= (\sum_{i \in Ij} g_i) + (\sum_{i \in Ij} h_i + \lambda)w_i = 0
$$
{% endraw %}
Solving for y we get:
{% raw %}
$$
w_j^* = \frac{-\sum_{i \in I_J} g_i}{\sum_{i \in I_J} h_i + \lambda}
$$
{% endraw %}

Intuitively, we can see the effect of the regularization parameter. A higher L2 penalty shrinks the weight estimates towards zero.

Plugging in, we can get the overall loss to be:

{% raw %}
$$
\tilde{L}^{(t)}_q = \frac{1}{2} \sum_{j =1}^T \frac{(\sum_{i \in I_j}g_i)^2 }{\sum_{i \in I_j}h_i + \lambda} + \gamma T
$$
{% endraw %}


Given this, the decision criteria for a split in the tree will be whichever split maximizes the reduction in loss

{% raw %}
$$
L_{\text{split}} = \frac{1}{2}\bigg[\frac{(\sum_{i \in I_L} g_i)^2}{\sum_{i \in I_L}h_i + \lambda} + \frac{(\sum_{i \in I_R} g_i)^2}{\sum_{i \in I_R}h_i + \lambda} - \frac{(\sum_{i \in I} g_i)^2}{\sum_{i \in I}h_i + \lambda}\bigg] - \gamma
$$
{% endraw %}

We can see how the gamma regularization parameter sets a threshold for how large the loss reduction must be.


## Intuition
What is the intuition behind using a taylor approximation and calculating a different estimator rather than simply following the gradients? To answer, we can notice that updating our estimates with nudges:
{% raw %}
$$
w_j^* = \frac{-\sum_{i \in I_J} g_i}{\sum_{i \in I_J} h_i + \lambda}
$$
{% endraw %}

Is similar to updating parameter estimates using newton's method:
{% raw %}
$$
x^{(i+1)} = x^{(i)} - \frac{\nabla f(x^{(i)})}{\text{Hess}f(x^{(i)})}
$$
{% endraw %}

Thus in Xgboost we are using information from the Hessian in order to tweak our estimates and approach the minimum of the loss function more quickly


## Other Features
Other  algorithmic advantages of Xgboost include using weighted quantile search for approximate best split and sparsity aware split finding for handling sparse data. 

[contact me](mailto:ethan_kim@college.harvard.edu)



