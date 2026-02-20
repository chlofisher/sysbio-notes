---
title: Loss Functions
tags:
    - machine-learning
---

A loss function is a function, $\cal{L}(\hat{y}, y)$ which maps a prediction, $\hat{y}$ and a corresponding true value, $y$, to a positive number, where lower indicates that the prediction was more accurate. Minimising this loss function is the goal of training a machine learning model.

Loss functions are designed to be easily differentiable with respect to $\hat{y}$, in order to facilitate backpropagation.

## Mean Square Error (MSE)
If $y \in \mathbb{R}^n$, the MSE is the $L_2$ norm of the difference between $y$ and $\hat{y}$, divided by $n$.
$$
\mathcal{L}_{\text{MSE}}(\hat{y}, y) = \frac{1}{n}||\hat{y} - y||^2
$$

This can be easily differentiated with respect to $\hat{y}$: 
$$
\nabla_{\hat{y}} \mathcal{L}_{\text{MSE}} = \frac{2}{n}(\hat{y} - y)
$$

## Categorical Cross-Entropy (CE)

Where $y$ is a probability distribution over a set of categories, the categorical cross-entropy measures the distance between two distributions.

$$
\mathcal{L}_{\text{CE}}(\hat{y}, y) = -\sum y_i \log(\hat{y}_i)
$$

$$
\frac{\partial \mathcal{L}_{\text{CE}}}{\partial \hat{y}_i} = -\frac{y_i}{\hat{y}_i}
$$

In the case where the true value is known to be $j$ (i.e., $y_i = \delta_{ij}$) this reduces to 
$$
\mathcal{L}_{\text{CE}}(\hat{y}, y) = -\log(\hat{y}_j)
$$
such that $\mathcal{L}_{\text{CE}}(\hat{y}, y) = 0$ if $\hat{y}_j = 1$, and  $\mathcal{L}_{\text{CE}}(\hat{y}, y) = \infty$ if $\hat{y}_j = 0$
