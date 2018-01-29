---
layout: default
### Smoothing methods for language models
---
#### Motivation

Statistical language model is estimating the probabality of next word given the history of the words i.e. probability of `n` word given previously seen `n-1` words. 
$$ 
P(w_n | w_1 ... w_n-1)
$$

Number of possible historical sequence of words for a given vocabulary grows exponentially based on length of history. 

Ex: If the size of vocabulary is `V`, then number of possible sequence of words for a history of length `n` is $$ V^n $$.

As the training corpus doesn't contain all possible historyi and due to sparseness of data, we need to adjust the probablity weight to avoid assigning zero proababilty to unseen and / or rare events. This is commonly known as smoothing. 

#### Setting / Terminology
* `V`: size of vocabulary
* `T`: Training dataset
* `H`: Holdout dataset
* `w`: Word in the vocabulary
* `h`: History / words from $$ w_1 ... w_n-1 $$ 
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
\sum\limits{w in V and w not seen after h} \frac{1}{C(h) + V} + \sum\limits{w in V and w seen after h}\frac{C(w,h)+1}{C(h) + V} = 1
$$



#### Reference
* []()
* []()
* []()

