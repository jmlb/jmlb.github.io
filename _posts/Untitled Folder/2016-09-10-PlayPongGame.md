---
layout: post
title: How to build a Neural Network for Playing Pong game
comments: true
date: 2016-09-10
tags: Reinforcement_learning Neural Network Pong_game gym
categories: MachineDeepLearning
---

<script src="https://gist.github.com/jmlb/e62d8672ef6074d771ae9649368a56c5.js"></script>

jmlb/e62d8672ef6074d771ae9649368a56c5


The goal is to train our agent to beat the computer at the Pong game

We are given:
+ a sequence of images (frames) representing each frame of the Pong game
+ an indication when we have won or lost the sequence
+ an agent that we control the action. It can only do 2 actions: move up or down.

# What you would need?
	+ openAI gym: it's a toolkit for developing RL algorithms
	+ Run `pip install -e .[atari]


# What we will do?
We gonna build a Neural Network that takes in each image and outputs a command to our AI to move up or down

The Neural Network will do:

1. Take images from the game and pre-process them: remove color, background and crop...
2. use NN to compute probability of moving up
3. sample from probability distribution and tell the agent to move up or down
4. If play is over: find out whether our agent won or lost
5. when episode has finished (someone gets to 21 points), pass the result through backprop to compute the gradient for our weights
6. After 10 episodes, sum up the gradient and move the weights in the direction of the gradient
7. repeat this process until our weights converge to the points where we beat the computer.


# The Learning
After seeing the result of our action and change our weights accordingly

## First setp of learning:
How does changing the output probability (of going up for example) affect my result of winning the round?

If \\( L  \\) is the value of our result, and \\( f \\) is the function that gives us the activations of our final layer, then

$$\frac{\partial L\_i}{\partial f\_j} = y\_{ij} - \sigma(f\_j)$$

where \\( \sigma() \\) is the sigmoid function.

I.e:

$$\frac{\partial L\_i}{\partial f\_j} = \text{true_label}(0 or 1) - $$


After one action (up or down), we don't really have an idea of whether or not that was the right action. So, we are going to cheat and treat the action we end up sampling from our probability as the correct action.

Our prediction for this round is going to be the probability of going up that we calculated.

We have the gradient per action:


Next step: figure out how we learn after the end of an episode (i.e when we or our approach miss the ball and someone gets a point).

We do this by computing the policy gradient of the network at the end of each episode. The intuition here is that if we won the round, we 'd like our network to generate more of the actions that led to us winning. Alternatively, if we loose, we are gonna try to generate less of these actions.

OpenAI Gym provides a `done` variable to tell us when the episode finishes.


## Next:
1. Compile all observations
2. compile all gradient calculation of an episode

We can apply our learnings over all the actions in the episode.

We want to learn in such a way that actions taken towards the end of an episode more heavily influence our learning than the action taken at the beginning.
It is called discounting.

We are going to take this weighting into account by discounting our rewards, such that rewards from earlier frames are discounted a lot more than rewards for later frames.

## Back propagation
We will then use back propagation to compute the gradient for our weights : i.e the direction we need to move our weights to improve.

They are 4 fundamental equations of back propagation:
$$ \delta ^L = \nabla \_a C  \dot  \sigma'(z^L) $$
$$\delta ^L = (( w ^{l+1})^T \delta ^{(l+1)} ) \dot \sigma'(z') $$
$$\frac{\partial C}{\partial b\_j ^l} = \delta\_j ^l$$
$$\frac{\partial C}{\partial w\_jk ^l} = a\_k ^{(l-1)} \delta\_j ^l$$

Our goal is to find \\( \frac{\partial C}{\partial w\_1} \\), the derivative of the Cost function with respect to the first layer weights, and \\( \frac{\partial C}{\partial w\_2} \\) the derivative of the cost function with respect to the 2nd layer.
The gradient will give the direction to move the weights.

$$ \frac{\partial C}{\partial w\_2} = a^{l\_2} \delta ^L $$

where \\( a^{l\_2} \\) is the activation of hidden layer

$$ \frac{\partial C}{\partial w\_1} = a^{l\_1} \delta ^{l\_2} $$
where \\( a^{l_1} \\) is the observation values

After we have finished `batch_size`  episodes, we finally update our weights for our Neural Network and implement our learning


To update our weights, we simply apply RMSProp : an algorithm for updating weights

$$ E[g^2]\_t = 0.9 \times E[g^2]\_{t-1} + 0.1  \times g\_t ^2$$

$$ \theta\_{t+1} = \theta\_t - \frac{\eta}{\sqrt{E[g^2]\_t + \epsilon}} g\_t $$


