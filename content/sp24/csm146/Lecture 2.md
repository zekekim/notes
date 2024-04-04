---
title: Lecture 2 - Perceptrons
draft: 
tags: 
date: 2024-04-03
---
## Issues in Machine Learning
### Representation
### Optimization
Solve for the best hypothesis, or 
$$
argmin_{h \in H} R^{emp}(h)
$$
### Evaluation
How can we gauge the accuracy of a hypothesis on unseen testing data?

## Perceptron Learning (1940s-1950s)
Based on neurons, with multiple inputs, a trigger, and an output. Modeled in 1943 by McCollough-Pitts. Below is the activation or affine function:
$$
w^Tx + b = \sum_{i}w_{i}x_{i} + b
$$
For simplicity sake, we are using binary inputs and outputs, $x, y \in \{0,1\}$.
Summary: add weighted inputs, add bias, then pass into a non linear function.
- Non linear function is called the sign function where anything less than 0 is -1 and anything greater than 0 is +1.
$$
H = {h|h:\mathbb{X} \to \mathbb{Y}, h(x) = sign\left( \sum_{d}w_{i}x_{i} + b\right) }
$$
$$
a = \sum_{d} w_{d}x_{d} + b
$$
$$
\hat{y} = sign(a)
$$
The Goal is to find the parameters $w_{1}\dots w_{D}, b$ with a training set of data to optimize for a loss function, 0/1 for now.
### Hyperplane? Go over this again
Define $\tilde{x}: \tilde{w}^T\tilde{x} = 0$ and $\tilde{w}$ s.t. they are orthogonal.
$$
\tilde{h}(\tilde{x}) = \tilde{w}^T\tilde{x}
$$
you are plotting these points on a hyperplane and the line in between distinguishes where
$$
\tilde{y}_{n}\tilde{h}(\tilde{x}) > 0, \leq 0
$$
### Example
Given a training example $(x_{n}, y_{n})$, how can we change $w$ s.t,
$$
y_{n} = sign(w^Tx_{n})
$$
So we have two cases:
- If $y_{n} = sign(w^Tx_{n})$, do nothing.
- If $y_{n} \neq sign(w^Tx_{n})$,
$$
w^{NEW} \leftarrow w^{OLD} + y_{n}x_{n}
$$
### Why is this a good method?

$$
y_{n}[(w+y_{n}x_{n})^Tx_{n}] = y_{n}w^Tx_{n} + y_{n}^2x_{n}^Tx_{n}
$$
So since we are adding a positive number, it is now possible that $y_{n}((w^{NEW})^T x_{n} > 0$
### Iterative Approach
- REPEAT
- Pick a data point $x_{n}$
- Compute $a = w^Tx_{n}$ using the current $w$
- If $ay_{n}>0$, do nothing. Else,
$$
w \leftarrow w + y_{n}x_{n}
$$
UNTIL converged. 

This is the Rosenblatt perception learning algorithm ~ 1957.
If the data is not linearly separable, you may not have a solution that currently classifies all examples. The max # of iterations terminates the loop.
- Thus you need to choose:
	- $MaxIter$: Hyperparameter
- How to loop over the data (heuristics)
	- Constant
	- Permuting once
	- Permuting in each iteration

### Properties
This is an **online** algorithm, which means it looks at one instance at a time.
Does this algorithm terminate (**convergence**)?
- If training data is not **linearly separable**, no, otherwise yes it will converge.
How long to convergence?
- Depends on the difficulty of the problem.
### Convergence
**Linear separability** of training set $\mathbb{D}$: There exists $w$ such that it gives zero empirical risk for the 0-1 loss-function over training set $\mathbb{D}$

For linearly separable dataset $D = \{(x_{i},y_{i})\} \subset \mathbb{R}^{D+1} \times \{-1.+1\}$ , with margin $\lambda$ and $R=max_{i}\mid\mid x_{i}\mid\mid$, then the perceptron algorithm converges in at most $\frac{R^2}{\lambda^2}$ updates.

There are different heuristics to add extensions and limits for different ideas in training.
## Logistic Regression
Hard to Soft decisions: Instead of predicting the class, predict the probability of an instance being in a class. The perceptron does not produce probability estimates.

>[!Observation]
> Even if the data is not linearly separable, we can infer a "trend" for confidence.

### Logistic Classification
Think about a sigmoid function. In perceptrons, we gave a "hard" output, which was $\in \{-1, +1\}$. Instead, we give a 'soft' output $\in [0,1]$.
Model:
$$
h_{w,b}(x) = p(y=1\mid x;b,w) = \sigma(a(x))
$$
and $\sigma$ stands for the sigmoid function:
$$
\sigma(a) = \frac{1}{1+e^{-a}}
$$
The activation function can really be anything, but we just take it from the perceptron activation, which is:
$$
a = \sum_{d} w_{d}x_{d} + b
$$
### Properties
- Bounded between 0 and 1
- Monotonically increasing, so
	- $\sigma(a) > 0.5$ positive, so classify as 1
	- $\sigma(a) < 0.5$ negative, so classify as 0
	- $\sigma(a) = 0.5$ is undecideable
- This has nice computational properties
- Nonlinear

Given a training data N samples/instances, we want to find values for $(w,b)$.
The Questions we want to ask are:
- What is the loss function?
- How do we find the solution? i.e. $argmin \frac{1}{N}\sum_{n}l(w_{n},b_{n})$
### Optimization Function
We are rewriting a hyperplane in $D$ dimensions as one in $D+1$ dimensions that passes through the origin.
$$
J(\theta) = - \sum_{n}\{y_{n}\log h_{\theta}(x_{n}) + (1-y_{n}) \log[1-h_{\theta}(x_{n})]\}
$$
The optimization is: Find $\theta$that minimizes $J(\theta)$. Also called KL-Divergence. Getting a distance from what you are finding from what you are looking for.
