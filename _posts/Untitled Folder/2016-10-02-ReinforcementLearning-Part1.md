---
layout: post
title: Reinforcement Learning Part 2/3
comments: true
date: 2016-10-02
tags: DQN Reinforcement-Learning Random-search Hill-climbing function-approximation
categories: MachineDeepLearning
---


# Part 1 | Part 2 | Part 3

This is our Part 2 of the series of posts on **Reinforcement Learning**.

In Part 2, I will talk about feature representation of action value-functions (*Q* functions) and use the car-pole game to illustratate some concepts. In Part 3, I will show an implementation of Deep Reinforcement Learning (DQN) and how we learn an agent to beat the computer at the pong game.

Let's dive in Part 2 !

## The cart pole game

In this game, a pole is attached by an un-actuated joint to a cart, which moves along a frictionless track. The system is controlled by applying a force of **+1** or **-1** (left or right) to the cart. The pendulum starts upright, and the goal is to prevent it from falling over. A reward of **+1** is given for every timestep that the pole remains upright. The episode ends when the pole is more than 15 degrees off the vertical, or the cart moves more than 2.4 units from the center.

<video id="pelican-installation" class="video-js vjs-default-skin" controls preload="auto" width="683" height="384" poster="/static/screencasts/pelican-installation.png" data-setup="{}">
<source src="/pics/posts_pics/RL_part1/training_episode_batch_video.mp4" type='video/mp4'> </video>
Click on the button to see a demonstration.


### What's our objective:

We have to get the pole to remain straight for 200 successive timesteps, i.e the agent must cumulate a total reward of 200 points during **an episode** (i.e the total duration of 200 timesteps). 

### What's the environment

There are 4 parameters (*aka* **observations**) that are recorded or that the agent knows about after each timestep. For example: the angle of the pole, the position of the cart, etc...
Those observations define the state of our agent.


## Linear function approximation
The function approximation consists in approximating the state value function $$V(s)$$ or the action value function $$Q(s,a)$$ into a parameterized combination of feature representation. The function approximation can be either linear type or non-linear. One way to generate a Non linear approximator is to use an Artificial Neural Network approximator. That will be the topic of [Part 3](). 

As of now, we will use a linear approximator with:

* 2 output to represent Q(s, a) function : Q(s, action=left) and Q(s, action=right),
* the input layer is 4 nodes with the observation values. The state $$s$$ is of the form: $$ s = \left( \begin{smallmatrix} 0.1 \\ 0.4 \\ -0.02 \\ 0.8 \end{smallmatrix} \right) $$


We define a vector weight $$\theta$$, (a $$4 \times 1$$ vector) where each weight is associated to one of the observations. Here are the steps:

1. At first, the weights are initialize with values between -1 and +1 (drawn from a normal distribution)
    
2. we compute the *log probability*: $$ \text{logProb} =  \theta^{\top} \times s = \sum_i \theta_i s $$

3. then, we can use the sigmoid function to convert to probability:  $$ prob =  \sigma(\theta^{\top} \times s) $$
	
4. depending on the probability $$prob$$, we ask the agent to perform an action: left or right if the probability is below 0.5

Therefore, in order to estimate how good a given set of weights is, we run several episodes and record the average cumulated reward per episode.


## Optimization

The question now is: **How can we select weights value so that we get the highest number of cumulated rewards?**.

### Random search algorithm
One technique to find the best $$\theta$$ values and improve our agent's performance is called **Random Search**. The idea is to continuously pick random values for the weights, and select the weights that result in the highest reward. Here, I implemented a simple Random Search, but it exists multiple random search algorithms that are more advance.

<script src="https://gist.github.com/jmlb/75bcc03368baec191cd434c112fd482c.js"></script>

![How many episodes are required before the agent reaches a total of 200 points.](/pics/posts_pics/RL_part1/output_3_1.png)



### Hill Climbing algorithm

Another method to get better weights values and improve our agent's performance, is called Hill Climbing method. Here is the main idea:

* Initialize the weights with values in the range -1 to 1
* after every single episode or every $$n$$ episodes, update the weights values by adding some noise to the weights: $$\theta_{\text{update}} = \theta_{\text{old}} + \text{noise}$$
* record the cumulated reward received by the agent before and after the weights update
* if the agent does not get more rewards with the updated weights (i.e the policy is not better), then keep the original weights

Hill climbing method has the advantage that it can bring the system closer to optimality. However, finding the best weights depends a lot on the initializaton: if the weights are badly initialized, i.e the weights values are very far from the optimum weights values, then adding noise will nto be sufficient to bring the system closer to optimality. Furthermore, the agent can also be trapped in a local maxima: in that case, the agent might have a high total reward, but it is not guaranteed to be the highest cumulated reward possible.

I posted on [openai.com](http://www.openai.com) a simple implemetation of the Hill Climbing algorithm: [check it out](https://gym.openai.com/evaluations/eval_zIOc1GZZQkaTE4dLVcdbdA). The graph below shows some stats about the agent performance. The agent is performing very poorly: most of the time, it gets a low cumulated reward < 200.

![Agent performance: most of the time the agent gets a low cumulated reward < 200](/pics/posts_pics/RL_part1/output_5_2.png)


The code below is an improved version of the Hill Climbing algorithm.
<script src="https://gist.github.com/jmlb/ee343019ed3a20698470afe79a1d0f9f.js"></script>

Here are the changes made to improve the agent's performance

+ instead of doing update on all the weights $$\theta = (\theta_1, \theta_2, ....\theta_i, .... )$$ simulatenously, now we do an iterative update, i.e a weight at a time. In that case, if the agent is not performing better after updating a single weight $$\theta_i$$, that weight is left unchanged.

+ now we have the option to update the weight only when the average cumulated reward over a batch of successive episodes is not improved. This is much more reliable (better accuracy and less variance) than using the cumulated reward after a single episode.

![frequency of cumulated reward after 1000 episodes](/pics/posts_pics/RL_part1/output_5_3.png)

So, the agent is able to cumulate 200 rewards for about 160 episdoes over 1000 episodes. With the previous implementation, it could reach 200 rewards for only 50 episodes.


That's it for now! Stay tune for Part 3 on RDN...that's gonna be fun.

Anyway, you should definitely check the website [openai.com](http://www.openai.com): they have plenty of games to try those methods as well as other RL algorithms.
