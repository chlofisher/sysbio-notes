---
title: Random Forests
tags:
    - machine-learning
    - supervised-learning
---

## Decision Trees

Decision trees can be used to solve classification problems on tabular data, where there can be a large number of features, not all of which necessarily relevant to the classification, and the relationship may be non-trivial. Decision trees can handle qualitative features and categorical data, unlike neural networks for which all inputs must be continuous values. Furthermore, the logic of a decision tree can easily be interpreted, as they mirror human decision-making processes and sort features into a hierarchy of importance. 

Decision trees can be trained using a greedy algorithm: recursively, at each node pick the feature which best separates the data, and split the data based on this. The greedy algorithm terminates when either all data points in a node belong to the same class, there are no more features left to split on, or the tree reaches a maximum depth. While this does not produce optimal trees (in general a difficult problem to solve), the weak trees produced are sufficient for aggregation and ensemble-averaging. 

A single decision tree is highly sensitive to small changes in the training data, and larger decision trees are prone to overfitting. 

## Random Forest

By combining several decision trees into a "forest" and having them vote on a consensus outcome, overfitting can be corrected and the variance can be decreased. Bagging (**b**ootstrap **agg**regat**ing**) is an ensemble-averaging meta-algorithm which can be applied to reduce overfitting. A large number of decision trees are created, and allowed to "vote" on the overall classification. By taking an average over many weak learners, the overall effectivenes sof the prediction is improved.

Instead of building trees over the entire training data set, "bootstrap" sets are created by sampling $n$ random data points with replacement. This means that each bootstrap set is the same length as the training data, but with repetitions. Training each decision tree on a different bootstrap set means that each tree is biased towards different elements of the training data. Further randomness is introduced by restricting each split to a subset of the features, preventing strong patterns from dominating the output.

Random forests maintain the advantages of decision trees over neural networks when applied to heterogeneous, tabular data, on which they are generally more effective and easier to train. However they are more difficult to interpret than individual trees. 
