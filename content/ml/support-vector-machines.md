---
title: Support Vector Machines
tags:
    - machine-learning
    - supervised-learning
    - linear-algebra
---

## Hard Margin SVM

Suppose we have $n$ observations:

$$
(\mathbf{x}_1, y_1), \dots, (\mathbf{x}_n, y_n)
$$

where $\mathbf{x}_i \in \mathbb{R}^p$ are $p$-dimensional features and $y_i \in \{-1, 1\}$ are labels indicating the class of the observations.

Let $\mathbf{w}$ be a normal (but not strictly normalised) vector such that $\mathbf{w} \cdot \mathbf{x} - b = 0$
defines a $(p-1)$-dimensional hyperplane in feature space. This hyperplane gives the decision boundary between the two classes of points (the hard margin SVM assumes that the two classes of points are linearly separable, i.e. a plane can be drawn which perfectly separates them). We want to find the plane separating the two classes with the maximal margin.

For a given decision plane, we can define two additional planes: 
$\mathbf{w} \cdot \mathbf{x} - b = \pm 1$. Let the margin be the distance between these two planes, $\frac{2}{||\mathbf{w}||}$.
We want to choose the smallest $\mathbf{w}$ such that these two planes are both also decision planes (i.e., maximising the size of the margin):

$$
\begin{aligned}
y_i = 1 &\implies \mathbf{w} \cdot \mathbf{x}_i - b \ge 1 \\
y_i = -1 &\implies \mathbf{w} \cdot \mathbf{x}_i - b \le -1
\end{aligned}
$$

In other words, we want to minimise $||\mathbf{w}||$ subject to $\ y_i(\mathbf{w} \cdot \mathbf{x}_i - b) \geq 1$, to find the plane which maximises the separation between the two classes.


## Soft Margin SVM

Often, it is not possible to draw a plane which linearly separates both classes of points. We can adapt the SVM algorithm to find the "least bad" plane by minimising the [[loss-functions|loss function]]:

$$
\mathcal{L}(\mathbf{w}, b) =  ||\mathbf{w}||^2 + C\left(\frac{1}{n}\sum_{i=1}^n \max(0, 1 - y_i (\mathbf{w}\cdot \mathbf{x_i} - b))\right)
$$

This loss function penalises points which lie on the wrong side of the margin, while also encouraging the margin the be as large as possible. The parameter $C$ defines the trade-off between these two goals.


## Example

```python
import numpy as np
from sklearn import svm
import matplotlib.pyplot as plt

# Generate some toy data
X = np.r_[np.random.randn(20, 2) - [2, 2], np.random.randn(20, 2) + [2, 2]]
Y = [0] * 20 + [1] * 20

# Fit the model
clf = svm.SVC(kernel='linear', C=1.0)
clf.fit(X, Y)

# Plotting logic
plt.scatter(X[:, 0], X[:, 1], c=Y, cmap=plt.cm.Paired, edgecolors='k')
ax = plt.gca()
xlim = ax.get_xlim()
ylim = ax.get_ylim()

xx = np.linspace(xlim[0], xlim[1], 30)
yy = np.linspace(ylim[0], ylim[1], 30)
YY, XX = np.meshgrid(yy, xx)
xy = np.vstack([XX.ravel(), YY.ravel()]).T
Z = clf.decision_function(xy).reshape(XX.shape)

ax.contour(
    XX, YY, Z,
    colors='k',
    levels=[-1, 0, 1],
    alpha=0.5,
    linestyles=['--', '-', '--'],
)
plt.show()
```

![[svm.png]]

| Strengths                                                | Weaknesses                           |
|----------------------------------------------------------|--------------------------------------|
| simple; geometrically intuitive                          | sensitive to outliers                |
| can be adapted to non-linear data using the kernel trick | only works for binary classification |
| works in high dimensions                                 |                                      |

