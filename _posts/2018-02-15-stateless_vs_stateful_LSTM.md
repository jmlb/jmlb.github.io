---
layout: post
title: LSTM- Stateless versus Stateful model in Keras 
category: flashcards
subcategory: dl
tags: DeepLearning learning NLP
summary: ""
img_post: 20180103_word_embedding.jpg
github-link: na
---

Given a big sequence as dataset. We split it into smaller sequences to construct the input matrix $$X$$ with shape:

```
X(nb_samples, timesteps, input_dim)
```

where `nb_samples` is the total number of sample in the dataset, `timesteps` is the sequence length, `input_dim` is the number of features in a sample.

Here are the steps:

* A Batch is build out of the dataset 
* feed the batch into the RNN.
* compute outputs
* average all the gradients
* Compute back propagation
* Weight update


### Can LSTM find dependencies between the sequences?
The answer is `YES`, only if the LSTM is **stateful**.


## Stateless LSTM
Most problems can be solved with stateless LSTM. In stateless mode, *long term memory* does not mean that the LSTM will remember the content of the previous batches. In stateless model, **Keras** allocates an array for the states of size `output_dim`. At each sequence processing, this state array is reset.


LSTM cell states are initialized for each training batch of dataset: this assumes each batch of datasetare **idd** to each other. This works well for text sentence

But for time series forecasting, **stateful** is better : there is not such cleart definition of breaks.


## Stateful model
In stateful model, a sample at index `i` in batch #` $$X_{i + bs}$$ will know the states of the sample $$i$$ in batch #0 ($$X_i$$). If a RNN is stateful, a complete `input_shape` must be provided, including `batch_size`.

When the LSTM network are stateful (`stteful=True`), then Keras implementation resets the network state after each training batch. It is used when we want t otreat consecutive batch as consecutive inputs.

Note that `reset_states` clears only the hidden state.

