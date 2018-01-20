---
layout: default
### Principle of Maximum Entropy
---

Consider if there are `n` events, and there is no addtional information available about those events, then the probability of each event is `1/n`. Although intuitive & obvious this is formally known as Principle of Indifference.

Given a 6 sided die, the probability of each number appearing on the face is $$\frac{1}{6}$$.

#### Model of a die
```python
import random
import itertools as it

def dice(prob):
    prob = [int(p * 100) for p in prob] # Convert probability to frequency
    events = []
    for (i, p) in enumerate(prob):
        events += it.repeat((i+1), p)
    return events

# Select a number at random
def roll(events):
    return random.choice(events)

# Average of list
def avg(prob, n):
    events = dice(prob)
    return sum([roll(events) for _ in range(n)]) / float(n)

def tries(prob, n):
	for _ in range(n):
		print(avg(prob, 100000))	

```

Average number appearing on the face is 3.5 

$$
\frac{1}{6} (1 + 2 + 3 + 4 + 5 + 6) 
$$

And empirically ~ 3.5 upon rolling a dice 100k time over 5 trials

```python

for _ range(5):
	print(avg([1/6. for _ in range(6)], 100000))

3.49981
3.49288
3.49387
3.50166
3.50544
```

#### Biased die

If a given die was biased, and the only information we had was average number appearing on the face, say 4.5, what would be ideal probability distribution ? 

Based on the information we have the known constraints are as follows

* $$ \sum\limits_{n=1}^{6} P_n = 1 $$
* $$ \sum\limits_{n=1}^{6} n * P_n = 4.5 $$


Some of the probability distributions that would fit the above constraints are as follows

* prob = [0, 0, 0, 0.5, 0.5, 0]
```python
prob = [0, 0, 0, 0.5, 0.5, 0]
tries(prob, 3)
4.50073
4.49989
4.50011
```

* prob = [0, 0, 0.25, 0.25, 0.25, 0.25]
```python
prob = [0, 0, 0.25, 0.25, 0.25, 0.25]
tries(prob, 3)
4.50188
4.49995
4.50024
```

* prob = [0.1, 0.1, 0.1, 0.1, 0.1, 0.5]
```python
prob = [0.1, 0.1, 0.1, 0.1, 0.1, 0.5]
tries(prob, 3)
4.4985
4.49614
4.50164
```

Although the probabilities agree with the constraints, it's still unreasonable that some events have no chance of occuring.

An ideal probability distribution should 
* Satisfy the constraints.
* Not give undue emphasis on any possibilities.
* Smooth (difference between probabilities between 2 events should vary by constant) & spread out.
* Doesn't make assumptions not warranted by constraints and data.

The measure we use to quantify the spread of distribution is `Shannon entropy` which measures uncertainty.

$$
E = - \sum\limits_{i=1}^{N} p_i * log(p_i)
$$

The goal is maximize the entropy while satisfying the contraints, thus capturing as much information we can from the data without making any assumption of what's unknown.

In case of our example the constraints would be 
* Probability of events sums to 1.
* Average number of dots that appear on face are `avg` (your favorite number between 1 to 6 here).
* Probability of each event is between 0 and 1.

$$
max (-p_i * \sum\limits_{i}^{N}p_i)
$$

such that 

$$
\sum\limits_{i=1}^{6} p_i = 1
$$

$$
\sum\limits_{i=1}^{6} i * p_i = a
$$

$$
\forall 0 <= p_i <= 1
$$

The above equation can be re-written and solved using [lagrange multipliers](https://en.wikipedia.org/wiki/Lagrange_multiplier).

Lagrange formulation

$$
max ( - \sum\limits_{1}^{6} p_i * log(p_i) + c_7 (\sum\limits_{i=1}^{6} p_i - 1) + c_8 (\sum\limits_{i=1}^{6} i * p_i - a) + \sum\limits_{i=1}^{6} c_i (abs(p_i) - p_i))
$$

Below is the sample code that implements this

```python
import numpy as np
from scipy.optimize import fsolve
import math

c = 8 # Number of constraints
avg = 3.5

# Lagrange function
def lagrange(P):
    print(P)
	# Entropy
    entropy = np.sum([-abs(p) * math.log2(abs(p)) for p in P[:-c]])
	# Contraints that sum of probabilties is 1
    cons_prob_sum_1 = P[-1] * (np.sum(P[:-c]) - 1.0) 	
	# Constrants for average number of dots on the face of the die
    cons_avg = P[-2] * (np.sum([p * i for (i, p) in zip(range(1,7), P[:-c])]) - avg)
	# Following constraints to ensure probability is between 0 and 1
    c1 = P[-3] * (abs(P[0]) - P[0])
    c2 = P[-4] * (abs(P[1]) - P[1])
    c3 = P[-5] * (abs(P[2]) - P[2])
    c4 = P[-6] * (abs(P[3]) - P[3])
    c5 = P[-7] * (abs(P[4]) - P[4])
    c6 = P[-8] * (abs(P[5]) - P[5])
    return entropy + cons_prob_sum_1 + cons_avg + c1 + c2 + c3 + c4 + c5 + c6

# Solving the lagrange
def partial_derivative(P):
    dL = np.zeros(len(P))
    h = 1e-3 # Need to tune this hyper-parameter for convergence and not get stuck in local minimas if any
    for i in range(len(P)):
        dP = np.zeros(len(P))
        dP[i] = h
        dL[i] = (lagrange(P + dP) - lagrange(P - dP)) / (2 * h)
    return dL

P = fsolve(partial_derivative, np.ones(14))
print(["%.3f" % p for p in P[:-c]])
print(np.sum(P[:-8]))

```

Upon setting average to `3.5` the best distribution we obtain is which is an ideal case

avg = 3.5
```python
[0.167, 0.167, 0.167, 0.167, 0.167, 0.167]
```

avg = 4.5
```python
[0.054, 0.079, 0.114, 0.165, 0.24, 0.347]
```

avg = 4.0
```python
[0.103, 0.123, 0.146, 0.174, 0.207, 0.247]
```

avg = 2.0
```python
[0.247, 0.207, 0.174, 0.146, 0.123, 0.103]
```

As you can see from the above distributions that there no emphasis on one / two events but the distribution is spread out and reasonbly smooth.

#### ToDo
* Proof for geometric and gausian distribution.
* Empirical examples for gausian distribution.
* Improve the language of this post.

#### Reference
* [Information Theory and Statistical Mechanics by E.T Jaynes](http://bayes.wustl.edu/etj/articles/brandeis.pdf)
* [Probabity dstribution and maximum entropy by Keith Conrad](http://www.math.uconn.edu/~kconrad/blurbs/analysis/entropypost.pdf)
* [Example code for Lagrange Multipliers](http://kitchingroup.cheme.cmu.edu/blog/2013/02/03/Using-Lagrange-multipliers-in-optimization/)

