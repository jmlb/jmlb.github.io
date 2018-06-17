
All things Sage 300…
The Road to TensorFlow – Part 10: More on Optimization
leave a comment »

      Rate This

Introduction
We’ve been playing with TensorFlow for a while now and we have a working model for predicting the stock market. I’m not too sure if we’re beating the stocking picking cat yet, but at least we have a good model where we can experiment and learn about Neural Networks. In this article we’re going to look at the optimization methods available in TensorFlow. There are quite a few of these built into the standard toolkit and since TensorFlow is open source you could create your own optimizer. This article follows on from our previous article on optimization and training.

Weaknesses in Gradient Descent
Gradient Descent has worked for us pretty well so far. Basically it calculates the gradients of the loss function (the partial derivatives of loss by each weight) and moves the weights in the direction of lowering the loss function. However finding the minimums of a complicated nonlinear function is a non-trivial exercise and compound this with the fact that a lot of the data we are feeding in during training is very noisy. In our case the stock market historical data is probably quite contradictory and is probably presenting a good challenge to the training algorithm. Here are some weaknesses these other algorithms attempt to address:

Learning rate. We have one fixed learning rate (how far we move in the direction of the sign of the gradient). We added an optimization to reduce this learning rate as we proceed, but we use the same learning rate for everything at each step. But some parts of our weight matrix may be changing quickly and other parts remaining close to constant. So perhaps use a different learning rate for each weight/bias and vary it by how fast it’s moving and whether it’s moving consistently in the same direction.
Getting stuck in local minimums or wandering around plateaus. Are we getting stuck in a local minimum which is much worse than the global minimum we would like to find? How can we power past global minimums and continue to the real goal?
TensorFlow Optimizers
The optimizers included with TensorFlow are all variations on Gradient Descent. There are many other optimizers that people use like simulated annealing, conjugate gradient and ant colony optimization but these tend to either not work well with multi-layer Neural Networks or don’t parallelize well to run on GPUs or a distributed network or are far too computationally intensive for large matrices. We added to the code all the optimizers and you just uncomment the one that you want to use.

    # optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(loss, global_step=global_step)

    # optimizer = tf.train.AdadeltaOptimizer(starter_learning_rate).minimize(loss)

    # optimizer = tf.train.AdagradOptimizer(starter_learning_rate).minimize(loss)     # promising

    # optimizer = tf.train.AdamOptimizer(starter_learning_rate).minimize(loss)      # promising

    # optimizer = tf.train.MomentumOptimizer(starter_learning_rate, 0.001).minimize(loss) # diverges

    # optimizer = tf.train.FtrlOptimizer(starter_learning_rate).minimize(loss)    # promising

    optimizer = tf.train.RMSPropOptimizer(starter_learning_rate).minimize(loss)   # promising
 

Perhaps it would be less hacky to make this a parameter to the program, but we’ll leave that till we need it.

Let’s quickly summarize what each optimizer tries to accomplish:

MomentumOptimizer: If gradient descent is navigating down a valley with steep sides, it tends to madly oscillate from one valley wall to the other without making much progress down the valley. This is because the largest gradients point up and down the valley walls whereas the gradient along the floor of the valley is quite small. Momentum Optimization attempts to remedy this by keeping track of the prior gradients and if they keep changing direction then damp them, and if the gradients stay in the same direction then reward them. This way the valley wall gradients get reduced and the valley floor gradient enhanced. Unfortunately this particular optimizer diverges for the stock market data.
AdagradOptimizer: Adagrad is optimized to finding needles in haystacks and for dealing with large sparse matrices. It keeps track of the previous changes and will amplify the changes for weights that change infrequently and suppress the changes for weights that change frequently. This algorithm seemed promising for the stock market data.
AdadeltaOptimizer: Adadelta is an extension of Adagrad that only remembers a fixed size window of previous changes. This tends to make the algorithm less aggressive than pure Adagrad. Adadelta seemed to not work as well as Adagrad for the stock market data.
AdamOptimizer: Adaptive Moment Estimation (Adam) keeps separate learning rates for each weight as well as an exponentially decaying average of previous gradients. This combines elements of Momentum and Adagrad together and is fairly memory efficient since it doesn’t keep a history of anything (just the rolling averages). It is reputed to work well for both sparse matrices and noisy data. Adam seems promising for the stock market data.
FtrlOptimizer: Ftrl-Proximal was developed for ad-click prediction where they had billions of dimensions and hence huge matrices of weights that were very sparse. The main feature here is to keep near zero weights at zero, so calculations can be skipped and optimized. This algorithm was promising on our stock market data.
RMSPropOptimizer: RMSprop is similar to Adam it just uses different moving averages but has the same goals.
Neural networks can be quite different and the best algorithm for the job may depend a lot on the data you are trying to train the network with. Each of these optimizers has several tunable parameters. Besides initial learning rate, I’ve left all the others at the default. We could write a meta-trainer that tries to find an optimal solution for which optimizer to use and with which values of its tunable parameters. You would want a quite powerful distributed set of computers to run this on.

