---
layout: default
---

### Regularization

This post is to provide a setting to explore one of the fundamental concepts in statistical learning - regularization.

#### Problem Setting

The goal of supervised learning is to find a function $$ f $$ in the hypothesis space $$ H $$ such that it approximately fit data points $$ S $$

$$

S = \{ (x_1, y_1), (x_2, y_2), ... , (x_n, y_n) \}

$$

Where, $$ x_i $$ is a input data point. $$ y_i $$ is input data point label (in case of classification problem) or $$ y_i \in R $$ (in regression setting)

In order to fit the function $$ f $$ to the data points $$ S $$ we rely loss functions $$ V $$, which measures the cost of making a a mistake while predicting $$ f (x_i) $$

Some of the exmaples of `Regression Loss function` are
* `Squared Loss`: $$ V(f(x_i), y_i) = (f(x_i) - y_i)^2 $$ 
* `Absolute Loss`: $$ V(f(x_i), y_i) = \| f(x_i) - y_i \| $$ 
* `Vapnik Loss`: $$ V(f(x_i), y_i) = (\|f(x_i) - y_i\| - \epsilon)_{+} $$  

Some of the exmaples of `Categorical or 0/1 Loss function` are
* `Squared Loss`: $$ V(f(x_i), y_i) = (1 - y_i f(x_i))^2 $$ 
* `0 - 1 Loss`: $$ step-func(- y_i f(x_i)) $$ 
* `Hinge Loss`: $$ V(f(x_i), y_i) = max(0, ( 1 - y_i f(x_i)) $$  

Risk is expected loss, $$ R(f) = \textbf{E} L(f(x), y) $$, or the error that the $$ f(x) $$ will make in future.


* `Expected Risk`: Loss averaged over unknown distribution of data. $$ I[f] = \int\limits_{X, Y} V(f(x), y) p(x, y) dx dy$$
* `Emperical Risk`: Loss measure over a given set of data $$ I_n[f] = \frac{1}{N} \sum\limits_{n=1}^{N} V(f(x), y)$$

Usually when the size of dataset becomes inifinite emperical error get closer to expected error. 

$$ I_n[f] \to I[f] $$ when $$ n \to \infty $$

So our assumption and hope is that the empirical error is a proxy of expected error.

#### Learning algorithm

The job of a learning algorithm in supervised setting is to find a function that minimizes the emperical error over a given dataset, this is commonly referred as `Emperical Risk Minimization`.

$$ 
f_s = argmin_{f \in H} I_s[f] 
$$

Imagine that the size of the hypothesis space is infinite, and there can be many functions in the hypothesis space that perfectly fit the given data points and fail to fit the data point that we would see in the future. 

Our goal would be to place a restriction on the search of function in the hypothesis space such that there is a reasonable guarantee that $$ f_s $$ is approximate representation of actual function. This is called regularization. 

There are two ways of regularization,
* `Direct way` or `Ivanov regularization`: Minimize the emperical error while subjecting the function $$ f $$ to satify a constraint.
$$ 
min_{f \in H} I_n[f] \mid R(f) \leqslant A
$$

* `Indirect way` or `Tikhonov regularization`: This is the most commonly used regularizer. Here we minimize the emperical error while adding the regularizer of the function to the emperical error.
$$ 
f_n = argmin_{f \in H} ( I_n[f] + \gamma R(f) )
$$

Where $$ R(f) $$ is a regularizer that penalizes the complexity of the function. If the function is more complicated the regularizer returns high value for that function thus keeping overfitting in check while generalizing $$ f $$.





