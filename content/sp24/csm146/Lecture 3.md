---
title: Lecture 3 - Logistic Regression
draft: 
tags: 
date: 2024-04-08
---
## Cross-Entropy Loss
Find how close your prediction $h_{\theta}(x)$ as a surrogate for $p(y \mid x)$, where we want to calculate $\hat{\theta}$ which is defined as
$$
\hat{\theta} = argmin J(\theta) = argmin\left\{  -\frac{1}{N} \sum_{n}\{y_{n}\log h_{\theta}(x) + (1-y_{n})\log(1-h_{\theta}x)\} \right\}
$$
## Optimization

>[!Question]
>How do we minimize a function $f$, where this function is the loss function? >You use **gradient descent**, which utilize partial differentials in order to find the change in each component for each vector function component.

When a function is well-behaved, then there is only one minimum.
Analytically, $\nabla J(\theta) = 0$ can be solved - many times no analytical solution or different computationally.
- We will do optimization at a very high level.
### What is gradient descent?
```
Start at a random point
REPEAT
	determine a descent direction
	choose a step size
	update
UNTIL stopping criterion is satisfied // translating to a final error
```

>[!Question]
>When will this "work"? i.e. finding a global minimum.
>In a convex function we can get to a global minimum or at least close to it using gradient descent. Ex. Least Squares, Ridge Regression, and Logistic Regression are all convex.
>
>Lots of neural networks lead to however, non-convex optimization. Convergence of gradient descent is to a "local" optimal dependency.

## Convex Functions
A function $f(x)$ is convex if geometrically proved that
$$
f(\lambda a + (1- \lambda) b) \leq \lambda f(a) + (1 - \lambda)f(b)
$$
$$
0 \leq \lambda \leq 1
$$
This is, for every chord present, or line delineated by two points on the function, it will be greater than or equal to the function between those values of input.

A better definition for this is if $f(w)$ is convex,
$$
f''(w) \geq 0
$$
meaning $f$ is twice differentiable and not inverted.
Examples:
$$
\begin{align}
f(w) &= aw + b \\
f(w) &= w^2  \\
f(w) &= e^w  \\
f(w) & = \frac{1}{w}, w \geq 0
\end{align}
$$
Some nonconvex functions:
$$
\begin{align}
f(w) &= \cos(w) \\
f(w) &= e^w - w^2 \\
f(w) &= \log(w)
\end{align}
$$
The last, $\log(w)$ being a concave function.
## Multi-variate functions
Given an angle $\theta$ defined as
$$
\theta = \begin{bmatrix}
b \\
w_{1} \\
\vdots \\
w_{2}
\end{bmatrix}
\in R^{\mathbb{N}+1}
$$
you are trying to find $f$ where
$$
f: \mathbb{R}^{D+1} \rightarrow \mathbb{R}
$$
$$
\forall \theta_{1},\theta_{2}, f(\lambda \theta_{1} + (1-\lambda)\theta_{2}) \leq \lambda f(\theta_{1}) + (1- \lambda)f(\theta_{2})
$$
We want this $f$ so that we can perform gradient descent in order to minimize this loss function.

Instead of representing this graphically, how can we find convexity using the case of second-order derivatives?

>[!Definition]
>Positive semi-definite matrices: A symmetric matrix $H$, where $H_{ij} = H_{ji}$ is positive semi-definite if $\forall z, z^THz \geq 0$.

Consider the function $f: \mathbb{R}^{D+1} \to \mathbb{R}$
$$
f(w), \nabla f(w) = \begin{bmatrix}
\frac{\partial f}{\partial w_{1}} \\
\vdots \\
\frac{\partial f}{\partial w_{D+1}}
\end{bmatrix}
$$
Now we define the Hessian. I don't wanna write all that so look it up.
A sufficient condition for convexity is that $H$, or the Hessian matrix, is positive semi-definite.
- A matrix $H$ is positive semi-definite iff
 $$
z^THz=\sum_{j,k} H_{j.k}z_{j}z_{k} \geq 0, \forall z
$$
Example:
Given a function defined as
$$
\begin{align}
f &: \mathbb{R}^2 \to \mathbb{R} \\
f(w) &= w^2_{1} + 2w^2_{2} \\
\end{align}
$$
Now find the gradient, and Hessian
$$
\begin{align}

\nabla f(w) &= \begin{bmatrix}
\frac{\partial f}{\partial w_{1}} \\
\frac{\partial f}{\partial w_{2}}
\end{bmatrix}  = \begin{bmatrix}
2w_{1} \\
4w_{2}
\end{bmatrix} \\
\nabla^2f(w) &= \begin{bmatrix}
\frac{\partial^2f}{\partial^2w_{1}} & \frac{\partial^2f}{\partial w_{2} \partial w_{1}} \\
\frac{\partial^2 f}{\partial w_{2} \partial w_{1}} & \frac{\partial^2f}{\partial^2w_{2}}
\end{bmatrix}  = \begin{bmatrix}
2 & 0 \\
0 & 4
\end{bmatrix} \\
\end{align}
$$
And finally to check for p.s.d,
$$
z^T Hz = \begin{bmatrix}
z_{1} & z_{2}
\end{bmatrix}
\begin{bmatrix}
2 & 0 \\
0 & 4
\end{bmatrix}
\begin{bmatrix}
z_{1} \\
z_{2}
\end{bmatrix}
=\begin{bmatrix}
2z_{1} & 4z_{2}
\end{bmatrix}
\begin{bmatrix}
z_{1} \\
z_{2}
\end{bmatrix}
= 2z_{1}^2 + 4z_{2}^2 \geq 0
$$
Therefore it is convex.

## Gradient Descent
Calculate the gradient for a function vector, where:
$$
\nabla f(w) = \begin{bmatrix}
\frac{\partial f}{\partial w_{1}} \\
\vdots \\
\frac{\partial f}{\partial w_{D+1}}
\end{bmatrix}
$$
and update the angle by:
$$
\theta^{t+1} \leftarrow \theta t - \eta \frac{\partial f}{\partial \theta} \mid_{\theta=\theta^t}
$$
where $\eta$ is the step size.
### Choosing the right step size is important
Smaller step-size means too small, Larger step size is too unstable
- Can be done by line search or use appropriate choice based on a convergence analysis
## Gradient Descent Update for Logistic Regression
Now for the function
$$
\sigma(a) = \frac{1}{1+e^{-a}}
$$
Where we have
$$
\begin{align}
\frac{d\theta(a)}{da} &= \frac{d}{da}(1+e^{-a})^-1  \\
&= -\frac{(1+e^{-a})'}{(1+e^{-a})^2} \\
&= \frac{e^{-a}}{(1+e^{-a})^2} \\
&= \sigma(a)[1-\sigma(a)]
\end{align}
$$
Now we find:
$$
\frac{\partial}{\partial \theta}\log h_{\theta}(x) = \frac{1}{h_{\theta}(x)} * \frac{\partial}{\partial \theta} h_{\theta}(x) = \frac{1}{h_{\theta}(x)} * [1-h_{\theta}(x)] \frac{\partial}{\partial \theta}[\theta^Tx]
$$
We know that:
$$

\frac{\partial}{\partial \theta}(\theta ^T x) = \frac{\partial}{\partial \theta}\sum_{j}\theta_{j}x_{j} = x
$$
Therefore,
$$
\nabla_{\theta} \log[1-h_{\theta}(x)] = -h_{\theta}(x)x
$$
Now you have that:
$$
J(\theta) = -\sum_{n}\{y_{n}\log h_{\theta}(x_{n}) + (1-y_{n})\log[1-h_{\theta}(x_{n})]\}
$$
$$
\frac{\partial J(\theta)}{\partial \theta}= \sum_{n}\{h_{\theta}(x_{n}) - y_{n}\}x_{n}
$$
>[!Remark]
>Now $e_{n}= \{h_{\theta}(x_{n}) - y_{n}\}$ is called *error* for the $nth$ training sample.



