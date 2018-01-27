---
layout: default
### Smoothing methods for language models
---
Statistical language model is estimating the probabality of next word given the history of the words i.e. probability of `n` word given previously seen `n-1` words. 
$$ 
P(w_n | w_1 ... w_{n-1})
$$

Number of possible historical sequence of words for a given vocabulary grows exponentially based on length of history. 

Ex: If the size of vocabulary is `V`, then number of possible sequence of words for a history of length `n` is $$ V^n $$.

As the training corpus doesn't contain all possible historyi and due to sparseness of data, we need to adjust the probablity weight to avoid assigning zero proababilty to unseen and / or rare events. This is commonly known as smoothing. 

#### Reference
* []()
* []()
* []()

