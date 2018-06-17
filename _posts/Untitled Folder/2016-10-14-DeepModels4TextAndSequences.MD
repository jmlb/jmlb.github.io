---
layout: post
title: Deep Models for Text and Sequences - Deep Learning (@ Udacity)
comments: true
date: 2016-10-13
tags: NLP word2vec skipgram cbow embeddings 
categories: MachineDeepLearning
img_post: nlp_udacity.gif
---

## Intro
In a Natural language model, we look at the statistical characteristics of a sequence of words. From there, we then deduce probabilisitics predictions for a word (targt word) given a context or a context given a target word.
Typically, the context would be a set of words surrounding symmetricaly the target words.
We cn represent the statistical language model as probability distribution over sequence of words:

$$P(w_t | w_{t-2}, w_{t-1}) = \frac{\text{count}(w_{t-2}, w_{t-1}, w_t )}{\text{count}(w_{t-2}, w_{t-1})}$$
Neural probabilistic language models are traditionally trained using the maximum likelihood (ML). The principle is to maximize the probability of next word $$w_t$$ (for 'target') given the previous words $$h$$ (for history) in terms of a softmax function.

$$P(w_t | h) = \text{softmax} (score(w_t, h))$$

where $$score(w\_t, h)$$ computes the compatibility of word $$w_t$$ with the context $$h$$ (a dot product is commonly used). We train this model by maximizing its log-likelihood on the training set i.e maximizing:

$$ J_{ML} = \text{log} [P(w_t | h) ] = score (w_t, h) - \text{log}[\sum_{word w' \in V}  e^{-score(w'x, h)} ]$$


## Embedding
Embedding is different of hot encoding. Hot-encoding consists in using a vector of 0,1 to represent a word. Let's say our corpus dictionary has 3 words: [sun, moon and star]. Hot-encoding of those word would be sun=[1,0,0], moon=[0,1,0], start=[0,0,1] Imagine now you are dealing with millions of different words, as it is more likely the case in real cases. Then data sparsity would be a major problem in building language models.
An important concept of .... is word embedding. It is a representation of a word into high dimensional vectors, word embedding uses numbers to represent words.
Embeddings map words to small vectors called embeddings.

 For example, vector embedding representation of cat could be:
`W("cat") = [0.2, -0.4, 0.7, .....] `


### Similarity
A hot-vector representation does not give direct notion of similarity:

$$(w^{\text{hotel}})^T w^{\text{motel}} =? 0$$

A model that predicts the words context would treat similarly some words. For example: 
+ the "cat" purrs
+ This "kitty" hunts mice

Similar words occur in similar context. 

Words that are going to be close to each other if they have similar meaning or further apart if they don't.
Embeddings solve some of the sparsity problem of one-hot encoding. 

### Comparing embedding
In order to compare vectors , it is better to use the normalize cosine distance:

$$cosine = \frac{v_{\text{cat}} - v_{\text{kitty}}}{||v_{\text{cat}}|| \times ||v_{\text{kitty}}||}$$

Rather than $$ L2 = \| v_{\text{cat}} - v_{\text{kitty}} \|^2 $$. $$L2$$ is not relevant to the classification.


<svg width="220" height="160">   
<line x1="40" x2="100" y1="100" y2="20" stroke="black" stroke-width="3" stroke-linecap="butt"/>
<polyline points="92,18 105,28 110,5 " stroke="black" stroke-width="0" fill="black" />
<text x="120" y="15">kitten</text>
<line x1="40" x2="25" y1="100" y2="20" stroke="black" stroke-width="3" stroke-linecap="butt"/>
<polyline points="18,25 35,22 22,5 " stroke="black" stroke-width="0" fill="black" />
<text x="5" y="75">cat</text>
</svg>



### t-SNE
Visualize the embedding in 2D (PCA does not work because it looses information)

t-SNE technique is a visualization technique of a dimension reduction that conserves the relative distance between element.




## Train a logistic regression

### Skip-gram1
Here is an example of the procedure:

Let's take a series of sentences: 
The dog saw a cat 
the dog chased the cat
the cat climbed a tree

The vocabulary has 8 words.

1) order the words by number of counts and referenced by index
2) collect words and their frequencies to build an internal dictionary tree structures. Words that appear once or twice on a large corpus are uninteresting: typo, garbage and not enough data to make any meaningful training so ignore.

This means that the hidden layer of this model is really just operating as a look-up table.
This description is to lean relationship between pair of words.

<svg width="700" height="320">
<text x="5" y="140">'cat'</text>
<text x="90" y="20">embedding</text>
<line x1="45" x2="105" y1="135" y2="135" stroke="black" stroke-width="3" stroke-linecap="butt"/>
<polyline points="105,130 105,140 120,135 " stroke="black" stroke-width="0" fill="black" />   
<rect x="120" y="40" width="20" height="200" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>

<text x="110" y="270">$$v_{cat}$$</text>

<line x1="155" x2="225" y1="135" y2="135" stroke="black" stroke-width="3" stroke-linecap="butt"/>
<polyline points="225,130 225,140 240,135 " stroke="black" stroke-width="0" fill="black" /> 
<text x="250" y="138">W $$v_{cat}$$ + b</text>
<text x="250" y="158">linear model</text>

<line x1="370" x2="455" y1="135" y2="135" stroke="black" stroke-width="3" stroke-linecap="butt"/>
<polyline points="455,130 455,140 465,135 " stroke="black" stroke-width="0" fill="black" /> 
<rect x="465" y="40" width="20" height="200" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>

<text x="420" y="20">predicted</text>

<text x="610" y="20">target</text>
<rect x="630" y="40" width="20" height="200" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>
<line x1="490" x2="630" y1="135" y2="135" stroke="black" stroke-width="3" stroke-linecap="butt"/>

<text x="380" y="125">softmax</text>
<text x="500" y="125">cross entropy</text>

</svg>
Use backprop for update


There might be many vocabulary and computing cross-entropy could be very inefficient.

A trick is to use random sample the engative target: sampled softmax. $$P(w_t | h)$$ computes the compatibility of word $$w_t$$ with the context $$h$$


### CBOW Model (Continuous Bag of words Model)

<svg width="700" height="320">
<text x="5" y="70">x1</text>
<text x="5" y="140">x2</text>
<text x="5" y="210">x3</text>

<rect x="120" y="40" width="20" height="50" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>
<rect x="120" y="100" width="150" height="40" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>
<rect x="120" y="160" width="210" height="40" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>

<text x="120" y="210">W</text>

<text x="120" y="110">SUM (Projection)</text>


<rect x="465" y="40" width="20" height="200" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>

<text x="420" y="20">output layer</text>

<text x="420" y="20">word target</text>
</svg>
Predicting the word given the context:

input = $$w_{i-2}, w_{i-1}, w_{i+1}, w_{i+2}$$
output = $$w_i$$

CBOW is several time faster than skip-gram slightly better accuracy for the frequent words.
context is represneted by multiple words for a given target. For example, cat and tree coudl be context for climbed as the target word.

sum = (wx1 + wx2 + wx3)/c

### skip-gram Model
Predicting the context given the context, i.e $$P(w_i | w_{i-1})$$ conditionned on the previous word.
input = $$w_i$$
output = $$w_{i-2}, w_{i-1}, w_{i+1}, w_{i+2}$$

The context is not limited to the immediate context. Training instances can be created by skipping a constant number of words:

$$w_{i-3}, w_{i-4}, w_{i+3}, w_{i+4}$$

hence the name **skip-gram**
<svg width="700" height="320">
<text x="5" y="140">x2</text>


<rect x="120" y="40" width="20" height="50" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>
<rect x="120" y="100" width="150" height="40" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>
<rect x="120" y="160" width="210" height="40" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>

<text x="120" y="210">embedding</text>

<text x="120" y="110">hidden</text>


<rect x="465" y="40" width="20" height="200" stroke="black" fill="none" fill-opacity="0.5" stroke-opacity="0.8"/>

<text x="500" y="70">x1</text>
<text x="500" y="140">x2</text>
<text x="500" y="210">x3</text>

<text x="420" y="20">C outputs</text>
</svg>
**skip-gram** works well with small amount of training data.


### Examples

```python

vocabulary_size = 17
embedding_size = 3

words = ['peter', 'piper', 'picked', 'a', 'pick', 'of', 'pickled', 'peppers',
         'the', 'peck', 'of', 'pickled', 'peppers', 'that', 'peter', 'picked',
         'was', 'pickled', 'but', 'that', 'did', 'not', 'stop', 'peter', 
         'piper', 'from', 'having', 'his', 'pepper', 'party', 'which', 'was',
         'a', 'great', 'success', 'in', 'the', 'end']

data = [2, 4, 3, 8, 0, 9, 1, 5, 10, 0, 9, 1, 5, 7, 2, 3, 6, 1, 0, 7,
        0, 0, 0, 2, 4, 12, 0, 0, 0, 14, 13, 6, 8, 0, 0, 11, 10, 0]
```

Count words

```python
count = [['UNK', 12], ('pickled', 3), ('peter', 3), ('picked', 2),
         ('piper', 2), ('peppers', 2), ('was', 2), ('that', 2), ('a', 2), 
         ('of', 2), ('the', 2), ('in', 1), ('from', 1), ('which', 1), 
         ('party', 1)]
```

Dictionary : word ordered count with highest frequency. Add word to disctionary and with value increasing incdes
Reverse the dictionary
```python
dictionary = {'a': 8, 'peter': 2, 'from': 12, 'which': 13, 'that': 7, 'of': 9,
              'piper': 4, 'pickled': 1, 'picked': 3, 'in': 11, 'peppers': 5,
              'party': 14, 'the': 10, 'UNK': 0, 'was': 6}
reverse_dictionary = {0: 'UNK', 1: 'pickled', 2: 'peter', 3: 'picked', 
                      4: 'piper', 5: 'peppers', 6: 'was', 7: 'that', 8: 'a',
                      9: 'of', 10: 'the', 11: 'in', 12: 'from', 13: 'which',
                      14: 'party'}
```

Express text in terms of numbers. The number of hidden layer nodes define the size of our embedding space.
```python
# use random numbers in the 
# embeddings. I've used U[0, 1] instead of U[-1, 1] here. embeddings is initialized randomlyfirst and then learn to have meaningful vectors to perform some tasks.

embeddings=  [[0.2405, 0.5699, 0.8043, 0.1180, 0.9852, 0.9313, 0.5637,
               0.8384, 0.9017, 0.1290, 0.7044, 0.2764, 0.8042, 0.3825, 0.8190],
              [0.1124, 0.8865, 0.4237, 0.1810, 0.4041, 0.7244, 0.8120, 
               0.9904, 0.4499, 0.4873, 0.8553, 0.0004, 0.3007, 0.6602, 0.6995],
              [0.8375, 0.8115, 0.6336, 0.5824, 0.0701, 0.7339, 0.6280, 
               0.2340, 0.7524, 0.1584, 0.4082, 0.5751, 0.9363, 0.1113, 0.4448]]

tf.nn.embedding_lookup(embeddings, ids) = [[0.8043, 0.4237, 0.6336],
                                           [0.9852, 0.4041, 0.0701],
                                           [0.9017, 0.4499, 0.7524],
                                           [0.2405, 0.1124, 0.8375]]
# This represents your initial weightings for each word in the input record
# For learning, the embed values will get updated by the backprop

```

embedding look_up
embedding shape (size of vocabulary, number of features)

word = W[id_word] = [a0, a1, a2, a3]

$$ W = \left( \begin{matrix} [- & - & - & - & - & -] \\ [- & - & - & - & - & -] \\ [- & - & - & - & - & -] \\ [- & - & - & - & - & -] \\ \end{matrix} \right) = \left( \begin{matrix} \text{word1} \\ \text{word2} \\ \text{word3} \\ \text{word3} \\ \end{matrix} \right)$$


Train the NN


+ `window_size` determines how far forward and backyard to look for context word.

We will use the embeddings to predict the context of the word: the context will be the words around.
A reasonable value for hidden layer node is 10 to 100s.

context = window of words to the left and to the right of a target word.

"the quick brown fox jumped over rthe lazzy dog"

window_size=1

([the, brown], quick), ([quick, for], brown), .......
(context, target) pairs

recall that skipgram tries to predict context word from target so dataset is:
(quick, the), (quick, brown), (brown, quick), (brown, fox)   of (input, output) pairs

The objective function is defined as the entire dataset but typically we use stochastic SGD using one example at time or a "mini-batch" of batch_size examples (16 <= batch_size <= 512)


+ u = W(1) X
+ v = W(2) u
+ prob = softmax(v)

#### Next
We select `num_noise` number of noisy (contractive) examples by drawing from noise distribution P(W). For simplicity, `num_noise=1` and we select "sheep" as a noisy example.

span = [w0, w1, w2], where w1 is target

+ embedding size = size hidden layer
+ skip_window = how many words to consider left and right
+ num_skips = how many time to reuse a target