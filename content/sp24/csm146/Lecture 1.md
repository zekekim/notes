---
title: Lecture 1 - Introduction
draft: 
tags: 
date: 2024-04-01
---
**Probabilistic Data Model**: There is an **unknown** probability distribution called $p$ called data-generating distribution over the instances.
**Performance**: measure through a **loss function**
$$
\begin{align}
l(h(x),y) \\
\hat{y}_xy \rightarrow \mathbb{R} \\
\end{align}
$$
**Technical formulation**:
- Set of possible instance $X$
- Set of possible labels $Y$
- Unknown target function $f: X \rightarrow Y$
- Find a prediction for $y$ defined by $h(x)$

>We care about generalization on **unknown/unseen data**. 
>The assumption is that your unseen data is coming from the same distribution as the seen data.

## Empirical risk minimization
### Supervised learning
we aim to build a function $h(x)$ to predict the true value $y$ associated with the true value

**L2 Loss**: Mean Squared Error
**0/1 Loss**: Right is 0 and Wrong is 1
**Cross Entropy Loss**: Or logistic loss, captures the KL-divergence on a measure of probability distance. How far is $h(x)$ from the real value $y$ in terms of probability?

> Recap:
> We need a hypothesis class $H$, a loss function, and a criterion to find $h \in H$ to best predict from data

### Measure how good our hypothesis $h$ is 
Think about minimizing your risk on unseen data.
**Risk/Expected test loss**: assume we know the true distribution of data $p(x,y)$ the *risk* is

$$
R[h(x)] = \sum _{x,y} l(h(x), y)p(x,y)
$$

>Claim:
>Given an observation $x$, the optimal Bayes' classifier is given by:

$$
h^{opt}(x) = arg max_{y} p(y|x)
$$

You actually do not know the underlying distribution, so how do you solve this problem?
We cannot compute $R[h(x)]$ so we use *empirical risk/training error* given a training dataset $S$
$$
R^{emp}[h(x)] = frac{1}{N} \sum _n l(h(x_n), y_n)
$$
As the training set gets larger, the risk will get closer to its true value of how the probabilistic model fits. As with the **Law of Large Numbers** this convergence is true in probability.
As $N \rightarrow \inf$ , 
$$
R^{emp}[h(x)] \to R[h]
$$

## Pick a good hypothesis
A good hypothesis $h$ is one that minimizes risk/expected test loss $R[h(x)]$ but we cannot even compute it! Instead, pick $h$ that minimizes empirical risk/training error $R^{emp}[h(x)]$ This strategy is known as **empirical risk minimization**.
$$
min \hat{R} \to 0
$$

## Model selection and overfitting: Regression example
The goal is to find a function $h \in H$ that fits the data according to $l_2$ loss or quadratic loss.
$$
h(x) = \sum a_i(h)x^i 
$$
This is the polynomial function class. We can get an exact fit for the data if we increase the degrees to get the exact model for the **seen** data.
This is not good, it is **overfitting** the data to the seen data and will be too complex to capture **unseen** data. We want our model to be good enough on the test data and also good enough on the real data. 
