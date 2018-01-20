---
layout: default
### Principle of Maximum Entropy
---

Consider if there are `n` events, and there is no addtional information available about those events, then the probability of each event is `1/n`. Although intuitive & obvious this is formally known as Principle of Indifference.

Given a 6 sided die, the probability of each number appearing on the face is `1/6`.

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













