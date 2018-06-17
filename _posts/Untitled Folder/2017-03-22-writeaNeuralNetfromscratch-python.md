---
layout: post
title: Write a Neural Network in python from scratch 
comments: true
date: 2017-03-22
tags: python Neural Network
categories: MachineDeepLearning
img_post: neuralnet_basic.jpg
github-link: na
---

Welcome to "*yet, another post on how to write a NN algo from scratch!!*". I actually wrote this piece as my personal notes on the subject.
We will start with [the math](), then we'll translate it into python code. And finally, I'll list the ressources that I found useful.


# Notations

Let's start by defining some notations:

<img src="/pics/posts_pics/nn_diagram01.png" width="400px">


* $$w_{kj}^l$$ : the weight edge from node  $$k$$  in layer  $$(l-1)$$  to node  $$j$$  in layer  $$l$$.
* $$b_{j}^l$$ : bias for node  $$j$$  in layer  $$l$$.
* $$a_{j}^l$$ : activation for node $$j$$  in layer  $$l$$.

# Feedforward:

feedforward is a weighted sum of the activations from the nodes in the previous layer:

* **the weighted input**: $$z_j ^l = \sum_k w_{kj} ^l a_k ^{(l-1)} + b_j ^l$$

* **Activation**: $$a_j ^l = \sigma(z_j ^l)$$

# Backpropagation:

The goal of the back-propagation is to compute $$\frac{\partial C}{\partial w}$$ and $$\frac{\partial C}{\partial b}$$, of the cost $$C$$ with respect to any weights $$w$$ and bias $$b$$ in the network:

$$C = \frac{1}{2n} \sum_x ||\hat{y}_x - y_x||^2$$

where $$n$$ is the total number of training examples, $$y_x$$ is the true/target value and $$\hat{y}_x$$ is the expected/predicted value.

For back propagation to be applied :

1) the cost function can  be written as an average $$\frac{1}{n} \sum_x C_x$$ over the cost function for all individual samples.

2) The cost function can be written as a function of the outputs of the neural net:

$$C = C(a_{L})$$

## The Error

we define the error in the $$j^{th}$$ neuron of layer $$l$$:

$$\delta_j ^l = \frac{\partial C}{\partial z_j ^l}$$

Intuition:

Let's say on neuron $$j$$, we add a bit of perturbation $$\Delta z_j ^l$$ to incoming input, i.e: $$z_j ^l = z_j ^l + \Delta z_j ^l $$.

<img src="/pics/posts_pics/nn_diagram02.png" width="400px">


The output with the perturbation is: $$a_j ^l = \sigma(z_j ^l + \Delta z_j ^l)$$

This change propagates through later layer causing the overall cost to change by:

$$\frac{\partial C}{\partial z_j ^l} \times \Delta z_j ^l$$

There is an *heuristic* sense in which $$\frac{\partial C}{\partial z_j ^l}$$ is a measure of the error in the neuron:

$$ \delta_j ^l = \frac{\partial C}{\partial z_j ^l}$$

Note that 

$$\sigma(z) = \frac{1}{1+e^{-z}}$$ and $$\frac{d \sigma}{dz} = \sigma(z)(1-\sigma(z))$$

The error of the output layer:

$$\delta_j ^L = \frac{\partial C}{\partial z_j ^L}$$

$$ \delta_j ^L = \frac{\partial C}{\partial z_j ^L} = \frac{\partial C}{\partial a_j ^L} \times \frac{\partial a_j ^L}{\partial z_j ^L} $$

where $$a_j ^L = \sigma(z_j ^L)$$ and $$\frac{\partial a_j ^L}{\partial z_j ^L} = \sigma'(z_j ^L)$$

Hence:

$$ \frac{\partial C}{\partial a_j ^L} = \frac{\partial}{\partial \hat{y}} \left( \frac{1}{2} (\hat{y} - y)^2 \right) = (\hat{y} - y)$$

So:

$$\delta _j ^L = (\hat{y} - y) \sigma'(z_j ^L)$$

* Error in layer $$l$$ as a function of error in layer $$(l+1)$$:

$$\delta_j ^l = \frac{\partial C}{\partial z_j ^l}$$

$$\delta_j ^{(l+1)} = \frac{\partial C}{\partial z_j ^{(l+1)}}$$

<img src="/pics/posts_pics/nn_diagram03.png" width="400px">

$$\delta_j ^l = \frac{\partial C}{\partial z_j ^l} = \frac{\partial C}{\partial z_k^{(l+1)}} \frac{\partial z_k ^{(l+1)}}{\partial z_j ^{l}}$$

We need to sum over all the $$k$$ node of layer $$(l+1)$$ that contributes to $$\delta_j ^{l}$$:

$$\delta_j ^{l} = \sum_k \frac{\partial C}{\partial z_k ^{(l+1)}} \times \frac{\partial z_k ^{(l+1)}}{\partial z_j ^l} = \sum_k \delta_k ^{(l+1)} \frac{\partial z_k ^{(l+1)}}{\partial z_j ^l} $$ 


What's $$z_k ^{(l+1)}$$ ?

$$z_k ^{(l+1)} = \sum_j w_{kj} ^{(l+1)} a_j ^l + b_k ^{(l+1)} = \sum_j w_{kj} ^{(l+1)} \sigma(z_j ^l) + b_k ^{(l+1)}$$



Hence:

$$\frac{\partial z_k ^{(l+1)}}{\partial z_j ^l} = w_{kj} ^{(l+1)} \sigma'(z_j ^l)$$

and:

$$\delta_j ^{l} = \sum_k \delta_k ^{(l+1)} w_{kj} ^{(l+1)} \sigma'(z_j ^l) $$ 

In vectorial form:

$$ \delta_j ^l = (w^{(l+1)}) ^T \delta^{(l+1)} .* \sigma'(z^l) $$

".*" means that we are doing element wise product



# How can we express the gradient with any weights:

$$w3 = \left| w_{11} w_{12} w_{13} \right|$$

and $$\frac{\partial J}{\partial w_3} = \left| \frac{\partial J}{\partial W_{11}} \frac{\partial J}{\partial W_{22}} \frac{\partial J}{\partial W_{13}}\right|$$


$$W_3$$ and $$\frac{\partial J}{\partial W}$$ have same form:

$$ \frac{\partial C}{\partial w_{jk} ^l} = \frac{\partial C}{\partial z_j ^l} .  \frac{\partial z_j ^l}{\partial w_{jk} ^l}  =  \frac{\partial C}{\partial z_j ^l}  . a_k ^{(l-1)}$$

Finally:

$$ \frac{\partial C}{\partial w_{jk} ^l} = \delta_j ^l a_k ^{(l-1)}$$

and weight update:

$$w_{ij} = w_{ij} - \eta \frac{\partial E}{\partial w_{ij}}$$


The 3 equations of interest:

$$ \delta_3 = np.mul(-(y -yhat), \sigma'(z3))$$

$$ \frac{\partial J}{\partial W2} = np.dot(a2.T, \delta3)$$

$$ \delta2 = np.dot(\delta3, W2 ^ T) \times \sigma'(z2)$$

$$ \frac{\partial J}{\partial W1} = np.dot(X.T, \delta2)$$




## Dead RELU
If z=0, a neuron gets clamped to zero forward pass then weights will get zero gradient ==> dead RELU

{% highlight python %}


	class NeuralNetwork:
	    def __init__(self, input_nodes, hidden_nodes, output_nodes, learning_rate):
	        # Set number of nodes in input, hidden and output layers.
	        self.input_nodes = input_nodes
	        self.hidden_nodes = hidden_nodes
	        self.output_nodes = output_nodes

	        # Initialize weights
	        self.weights_input_to_hidden = np.random.normal(0.0, self.hidden_nodes**-0.5, 
	                                       (self.hidden_nodes, self.input_nodes))

	        self.weights_hidden_to_output = np.random.normal(0.0, self.output_nodes**-0.5, 
	                                       (self.output_nodes, self.hidden_nodes))
	        self.lr = learning_rate
	        
	        #### Set this to your implemented sigmoid function ####
	        # Activation function is the sigmoid function
	        self.activation_function = lambda x: 1/(1+np.exp(-x))
	    
	    def train(self, inputs_list, targets_list):
	        # Convert inputs list to 2d array
	        inputs = np.array(inputs_list, ndmin=2).T
	        targets = np.array(targets_list, ndmin = 2).T
	        
	        #### Implement the forward pass here ####
	        ### Forward pass ###
	        # Hidden layer
	        hidden_inputs = np.dot(self.weights_input_to_hidden, inputs)
	        hidden_outputs = self.activation_function(hidden_inputs)
	        
	        # Output layer
	        final_inputs = np.dot(self.weights_hidden_to_output, hidden_outputs)
	        final_outputs = self.activation_function(final_inputs)
	        
	        ### Backward pass ###
	        # Output error
	        output_errors = targets - final_outputs
	        
	        # Backpropagated error
	        hidden_errors = np.dot(self.weights_hidden_to_output.T, output_errors)
	        hidden_grad = hidden_outputs * (1.0 - hidden_outputs)
	        
	        # Update the weights
	        self.weights_hidden_to_output += self.lr * np.dot(output_errors, 
	                                                          hidden_outputs.T)
	        self.weights_input_to_hidden += self.lr * np.dot(hidden_errors * hidden_grad, inputs.T)

	        
	    def run(self, inputs_list):
	        # Run a forward pass through the network
	        inputs = np.array(inputs_list, ndmin=2).T
	        
	        #### Implement the forward pass here ####
	        # Hidden layer
	        hidden_inputs = np.dot(self.weights_input_to_hidden, inputs)

	        hidden_outputs = self.activation_function(hidden_inputs)
	        
	        # Output layer
	        final_inputs = np.dot(self.weights_hidden_to_output, hidden_outputs)
	        print(final_inputs)
	        final_outputs = self.activation_function(final_inputs)
	        
	        return final_outputs

	def MSE(y, Y):
	    return np.mean((y-Y)**2)

	### Set the hyperparameters here ###
	epochs = 1000
	learning_rate = 0.001
	hidden_nodes = 200
	output_nodes = 1

	N_i = train_features.shape[1]
	network = NeuralNetwork(N_i, hidden_nodes, output_nodes, learning_rate)

	losses = {'train':[], 'test':[]}
	for e in range(epochs):
	    # Go through a random batch of 128 records from the training data set
	    batch = np.random.choice(train_features.index, size=128)
	    for record, target in zip(train_features.ix[batch].values, train_targets.ix[batch]['cnt']):
	        network.train(record, target)
	    
	    if e%(epochs/10) == 0:
	        # Calculate losses for the training and test sets
	        train_loss = MSE(network.run(train_features), train_targets['cnt'].values)
	        test_loss = MSE(network.run(test_features), test_targets['cnt'].values)
	        losses['train'].append(train_loss)
	        losses['test'].append(test_loss)
	        
	        # Print out the losses as the network is training
	        print('Training loss: {:.4f}'.format(train_loss))
	        print('Test loss: {:.4f}'.format(test_loss))
	        pass


	fig, ax = plt.subplots(figsize=(8,4))

	mean, std = scaled_features['cnt']
	predictions = network.run(val_features)*std + mean
	ax.plot(predictions[0], label='Prediction')
	ax.plot((val_targets['cnt']*std + mean).values, label='Data')
	ax.set_xlim(right=len(predictions))
	ax.legend()

	dates = pd.to_datetime(rides.ix[validation.index]['dteday'])
	dates = dates.apply(lambda d: d.strftime('%b %d'))
	ax.set_xticks(np.arange(len(dates))[12::24])
	_ = ax.set_xticklabels(dates[12::24], rotation=45)

{% endhighlight %}


