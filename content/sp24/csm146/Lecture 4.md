---
title: Lecture 4
draft: 
tags: 
date: 2024-04-10
---
## Regression
Predicting a continuous outcome variable vs. classification.
- Labels are real-valued instead of binary.
- Choose a prediction which is real valued, and measure its distance to the true label.
**Ex.** Appraising a house
- Given features such as square footage, fixed expense, etc. find the sale price of the property.
- Find the line where the errors are minimized along the line drawn.
### How do we evaluate the quality of a prediction?
- absolute difference: $l_{1}$ prediction
- square difference: $l_{2}$ prediction
We are to adjust the model such that the sum of the squared error is minimized.
$$
\begin{align}
|\hat{y}-y|^2 &= l(\hat{y};y)  \\
\hat{y} &= \theta^Tx = \sum_{i=1}^{D+1}\theta_{i}x^{(i)} \\
\hat{y}  &= w^Tx + b = \begin{bmatrix}
b & w_{1} & w_{2} & \dots w_{D}
\end{bmatrix} \begin{bmatrix}
1 \\
x_{1} \\
x_{2} \\
\vdots \\
x_{D}
\end{bmatrix} = \theta^T\tilde{x}
\end{align}
$$
The goal is to find $\theta$ s.t. $\theta = argmin J(\theta)$.
$$
\begin{align}
J(\theta) &= \sum_{n=1}^{N}|\hat{y_{i}}-y_{i}|^2 \\
\hat{y}_{i} &= \theta^Tx_{i} \\
D &= \{(x_{1},y_{1}), (x_{2}, y_{2}),\dots,(x_{N},y_{N})\}
\end{align}
$$
### How do we minimize RSS
$$
RSS = \sum_{i=1}^N|y_{i} - \hat{y}_{i}|^2
$$
Now how do we compute the gradient of this? And is this convex?
### $J(\theta)$ in new notations
We design a matrix input and target vector, where
$$
y-X\theta= \begin{pmatrix}
y_{1} \\
y_{2} \\
\vdots \\
y_{N}
\end{pmatrix} - \begin{pmatrix}
x_{1}^T\theta \\
x_{2}^T\theta \\
\vdots \\
x_{N}^T\theta
\end{pmatrix}
= \begin{pmatrix}
y_{1} - x_{1}^T\theta \\
y_{2} - x_{2}^T\theta  \\
\vdots \\
y_{N} - x_{N}^T\theta
\end{pmatrix}
$$
### Gradient and Hessian
Now the gradient of $J(\theta)$
$$
\frac{\partial J(\theta)}{\partial \theta} = \sum_{n}2(\theta^Tx_{n} - y_{n})x_{n}
$$
When twice differentiated, you would get a Hessian
$$
H=\sum_{n}2* \frac{\partial}{\partial \theta}[\theta^Tx_{n}-y_{n}]x_{n}^T = 2\sum_{n}x_{n}x_{n}^T
$$
$$
z^THz = 2 * \sum_{n} |z^Tx_{n}|^2 \ge 0
$$
Therefore it is a convex problem.
### Analytical solution of RSS
RSS may have a closed form solution for optimization.
$$
\nabla J(\theta) = 0
$$
If we can solve this we have found the optimal.
To solve
$$
\hat{\theta} = \text{argmin}_{\theta}J(\theta)
$$
$$
\nabla J(\theta)= \sum 2*(\theta^T x_{n}-y_{n})x_{n}=0
$$
and solve for theta. yielding
$$
\sum_{n}2x_{n}(x_{n}^T\theta-y_{n})= 2\left( \sum_{n}x_{n}x_{n^T} \right)\theta - 2\sum_{n}y_{n}x_{n}=0
$$
$$
\theta =\left(\sum_{n}x_{n}x_{n}^2\right)^{-1}\sum_{n}y_{n}x_{n}
$$
we can use the following definitions to substitute, where
$$
X^TX = \begin{bmatrix}
x_{1} & \dots & x_{N}
\end{bmatrix} \begin{bmatrix}
x_{1}^T \\
\vdots \\
x_{N}^T
\end{bmatrix}
=\sum_{n}x_{n}x_{n}^T
$$
$$
X^TY= \begin{bmatrix}
x_{1} & \dots & x_{N}
\end{bmatrix} \begin{bmatrix}
y_{1} \\
\vdots \\
y_{N}
\end{bmatrix}
= \sum_{n}y_{n}x_{n}
$$
$$
\hat{\theta}=(X^TX)^{-1} X^Ty
$$
$A^{-1}$ usually takes $O(m^3)$ to compute, and the implication is that if $D$ is large, then $(X^TX)$ $O(D^3)$ is large and expensive.
The problem is therefore that it is impractical for very large $D$ or $N$. Linear is needed.
- $O(ND^2)$ for matrix multiplication
- $O(D^3)$ for matrix inversion
The total complexity, in comparison, for Gradient Descent ($J$) is $O(ND)$ for each iteration.

>[!Implication]
>Even if we have a closed form solution, it is usually less computationally intensive to do gradient descent.

## Stochastic Gradient Descent
NOT a gradient descent. You are not actually trying to compute the true gradient.
If we do gradient descent, we still have O(ND) computation for each iteration.
SGD takes a single value from
$$
D = \{(x_{1},y_{1}),\dots,(x_{n},y_{n})\}
$$
randomly chosen from the dataset
$$
(x_{i(t)},y_{i(t)})
$$
and computing its contribution to the gradient
$$
g_{t} = (x_{i(t)}^T \theta^{(t)}-y_{i(t)})x_{i(t)}
$$
This is not the true gradient, where the true gradient sums over all data points.
The total complexity is $O(D)$ times the number of iterations.
The hope is that
$$
\mathbf{E} [\lvert \lvert \theta^{(t)} - \theta \rvert  \rvert ^2] \to 0
$$
SGD is by *Robbins-Munio*
SGD is said to perform better at times compared to gradient descent since it can take a better generalization of the data.
## Summary
True Gradient is
$$
\nabla J(\theta) = \sum_{n}(x_{n}^T\theta - y_{n})x_{n}
$$
SGD is
$$
(x_{i(t)}-y_{i(t)})x_{i(t)}
$$
- Batch Gradient Descent computes the exact gradient
- Stochastic gradient descent approximates the gradient with a single data point
