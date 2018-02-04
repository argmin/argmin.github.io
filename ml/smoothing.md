---
layout: default
### Smoothing methods for language models
---
#### Motivation

Statistical language model is estimating the probabality of next word given the history of the words i.e. probability of word given previously seen `n-1` words. 
$$ 
P(w_i | w_{(i-1)... (i-(n-1))})
$$

And, thus probability of forming a sentence is 

$$
P(S) = \prod_{i} P(w_i | w_{(i-1)... (i-(n-1))})
$$

Number of possible historical sequence of words for a given vocabulary grows exponentially based on length of history. 

Ex: If the size of vocabulary is `V`, then number of possible sequence of words for a history of length `n` is $$ V^n $$.

As the training corpus doesn't contain all possible historyi and due to sparseness of data, we need to adjust the probablity weight to avoid assigning zero proababilty to unseen and / or rare events. This is commonly known as smoothing. 

#### Setting / Terminology
* `V`: size of vocabulary
* `T`: Training dataset
* `H`: Holdout dataset
* `w`: Word in the vocabulary
* `h`: History / words from $$ w_{i-1} ... w_{i-n+1} $$ 
* `C(w, h)`: Frequency of word `w` and history `h` occuring together in the corpus.
* `C(h)`: Frequency of `h` in the corpus.

#### Naive Model

$$
P(w | h) = 
\Bigg\{ \begin{array}{cc}
0 & \mbox{if w was not seen after h} \\
\frac{C(w,h)}{C(h)} & \mbox{if w was seen after h}
\end{array}
$$

#### Laplace Smoothing
As seen earlier in the navive model, the probabilties for rare events are `0`, Laplace smoothing overcomes this problem taking some proabality mass from the seen events from the training corpus and assigning them unseen events shown in the equation below.

$$
P(w | h) = 
\Bigg\{ \begin{array}{cc}
\frac{1}{C(h) + V} & \mbox{if w was not seen after h} \\
\frac{C(w,h)+1}{C(h) + V} & \mbox{if w was seen after h}
\end{array}
$$

While shifting the probablity mass a point to note is that the probabilities are notmalized and they sum to 1
$$
\sum_\limits{w \notin training} \frac{1}{C(h) + V} + \sum_\limits{w \in training}\frac{C(w,h)+1}{C(h) + V} = 1
$$

What would happen when `V` is very large and the training data is limited as shown below, this is not an accurate depection of the distribution, and can assign high probability to rare sentences.

$$
\sum_\limits{w \notin training} \frac{1}{C(h) + V} \ggg \sum_\limits{w \in training}\frac{C(w,h)+1}{C(h) + V}
$$

One way to counter is to use add $$ 0 \leq \delta < 1 $$ smoothing instead of $$ 1 $$ as shown below.

$$
P(w | h) = 
\Bigg\{ \begin{array}{cc}
\frac{\delta}{C(h) + \delta * V} & \mbox{if w was not seen after h} \\
\frac{C(w,h)+\delta}{C(h) + \delta * V} & \mbox{if w was seen after h}
\end{array}
$$


#### Jelinek-Mercer smoothing / Interpolation
In this smoothing we will take an trigram language model as an example.
$$
P(S) = \prod_{i} P(w_i | w_{i-1}, w_{i-2})
$$

And, trigram relative frequency is given by
$$
P(w_3 | w_1, w_2) = f(w_3 | w_1, w_2) = \frac{C(w_1,w_2,w_3)}{C(w_1, w_2)}
$$

And since we already know that the many trigrams are unlikely to appear in the training set, we attempt to smooth the relative frequency by interpolating trigram, bigram, and unigram.
$$
P(w_3 | w_1, w_2) = \lambda_3 f(w_3 | w_1, w_2) + \lambda_2 f(w_3 | w_2) + \lambda_1 f(w_3)  
$$

where, $$ \lambda_3 + \lambda_2 + \lambda_1 = 1 $$

The interpolation model can be viewed as `HMM`, with $$\lambda_i$$ as transition probabilities. Here the training dataset is split into two parts `D` to estimate relative frequencies and `H` to estimate the weights i.e. $$ \lambda_i $$.

#### Katz smoothing
Consider the previous trigram relative frequency and the interpolation model. If the counts $$ C(w_1, w_2) $$ are sufficiently large then $$ f(w_3 | w_1, w_2) $$ is a better approximator for $$ P(w_3 | w_1, w_2) $$ and thus backoff formula is given by

$$
P(w_3 | w_1, w_2) = 
\Bigg\{ \begin{array}{cc}
f(w_3 | w_1, w_2) & \mbox{ if C(w_1, w_2, w_3) \geq K} \\
\alpha Q_T(w_3 | w_1, w_2) & \mbox{if 1 \leq C(w_1, w_2, w_3) < K} \\
\beta \hat{P}(w_3 | w_2) & \mbox{otherwise} \\
\end{array}
$$

where $$ Q_T $$ is a Good-Turing estimator and $$ \alpha $$ and $$ \beta $$ are the other hyperparameters needs to be determined based on $$ Q_T $$.
$$
Q_T(w_3 | w_1, w_2) = \frac{P_T(w_1, w_2, w_3)}{f(w_1, w_2)}  
$$

The bigram frequency can be recursively computed by

$$
P(w_3 | w_2) = 
\Bigg\{ \begin{array}{cc}
f(w_3 | w_2) & \mbox{ if C(w_2, w_3) \geq K} \\
\alpha Q_T(w_3 | w_2) & \mbox{if 1 \leq C(w_2, w_3) < K} \\
\beta f(w_3) & \mbox{otherwise} \\
\end{array}
$$


#### Estimating counts using held out dataset.


#### Good Turing



#### Reference
* [Statistical Methods for Speech Recognition. Jelinek](https://mitpress.mit.edu/books/statistical-methods-speech-recognition)
* [An empirical study of smoothing techniques for language modeling](http://u.cs.biu.ac.il/~yogo/courses/mt2013/papers/chen-goodman-99.pdf)
* [NLP Smoothing tutorial](https://nlp.stanford.edu/~wcmac/papers/20050421-smoothing-tutorial.pd()

