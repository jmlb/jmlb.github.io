---
layout: post
title: Policy gradient
comments: true
date: 2016-09-28
tags: reinforcement learning
categories: MachineDeepLearning
---

links:

Before we start with Policy gradient, there are a few concepts to know:

# feature-based representation
A feature describe a state using a vector features. Features are functions that map from states to real numbers (often 0 or 1) that capture important properties of the state.

Here are some example of features (applied to the pacman game):

* distance to the closest ghost
* distance to the closest dot
* 1/(distance to dot)^2


Q-state (s, a) can also be described with features. For example, *action: move closer to food*

## Function approximation

Using a feature representation, we can represent the action-value function by a parametrized function approximation, IN simpler wording, we write a Q function for any state using a few weights. 

{% raw %}
$$ \hat{Q}(s, a, w) = w\_1 f\_1(s, a) + w\_2 f\_2(s, a) + .... + w\_n f\_n(s, a) \approx Q\_{*}(s, a) or Q_{\pi}(s, a) $$
{% endraw %}

(it can also include quadratic term.)
The approximation could be provided by a Neural Network with the weights being the parameters, or simply a linear weighting of features. 

* **Advantages**: our experience is summed up in a few powerful numbers.
* **disadvantages**: states may share features but be very different in value

Q-learning with linear Q-function:

{% raw %}
$$ \text{transition} = (s, a ,r, s') $$
{% endraw %}

{% raw %}
$$ \text{difference} = [r + \gamma \text{max}\_{a'} Q(s', a')] - Q(s, a) $$
{% endraw %}

The first term is the exact Q's (Bellman)
{% raw %}
$$ Q(s, a) \leftarrow Q(s, a) + \alpha [difference] $$
{% endraw %}


{% raw %}
$$ w\_i \leftarrow w\_i + \alpha [difference] f\_i(s, a) $$
{% endraw %}

This is an approximation of the Q's

Case exemple: 

* if \\( f\_i(s, a) = 0 \\), \\(w\_i \\) is not updated  ===> good

* if \\( f\_i(s, a) = \pm 1 \\), \\( w\_i \\) is updated  ===> good

Note that we need all the features to be on the same scale [0, 1] or [+1, -1]


Intuitive interpretation: 

* adjust weights of active features
* if something unexpectidely bad happens, blame the feature that were **ON**: disprefer all states with that states features.


## Exemple of Q-learning in action with function approximation:
{% raw %}
$$ Q(s, a) = 4.0 f\_{\text{DOT}}(s, a) - 1.0 f\_{\text{GST}}(s, a) $$
{% endraw %}


The 2 features are \\f\_{\text{DOT}} \\) which is the feature related to the distance to the dot, and \\f_{\text{GST}} \\) which is the feature related to the distance to the ghost.

Let's assume, we take the action NORTH:
{% raw %}
$$ f\_{DOT} (s, NORTH) = 0.5 $$
$$ f\_{GST}(s, NORTH) = 1.0 $$
{% endraw %}


The current Q(s, NORTH) = 0.5 * 4 - 1 * 1 = 2-1=1

With action=North resulting in state s', we receive an instant reward \\(r=-500 \\).

The \\(Q(s', .) =0 \\)

Sample  estimate: \\( r + \gamma \text{max}_{a'} Q(s', a') = -500 \\)

Difference: [sample estimate - current] = -500 -1 

{% raw %}
$$ w\_{DOT} \leftarrow 4.0 + \alpha [-501] * 0.5 $$
{% endraw %}
{% raw %}
$$ w\_{GST} \leftarrow -1.0 + \alpha [-501]*1.0 $$
{% endraw %}

Update q-function:

{% raw %}
$$ Q(s, a) = 3.0 f\_{DOT} (s, a) - 3.0 * f\_{GST} (s, a) $$
{% endraw %}

We are now more afraid of the ghost and little less easger to collect food pellets.


Imagine we had only one point \\( x \\), with feature \\( f(x) \\), target value \\( y \\), and weights \\( w \\).

{% raw %}
$$ error(w) = \frac{1}{2} \left( y - \sum\_k w\_k f\_k(x) \right)^2 $$
{% endraw %}

{% raw %}
$$ \frac{\partial error(w)}{\partial w\_m} = - \left( y - \sum\_k w\_k f\_k(x) \right) f\_m(x) $$
{% endraw %}

{% raw %}
$$ w\_m \leftarrow w\_m + \alpha (y -sum w\_k f\_k(x)) f\_m(x)$$
{% endraw %}
{% raw %}
$$ w\_m \leftarrow w\_m + \alpha [error] f\_i(s, a)$$
{% endraw %}


## Approximate $$Q$$ update explained:

{% raw %}
$$ w\_m \leftarrow w\_m + \alpha \left[ r + \gamma \text{max}_a Q(s', a') - Q(s, a) \right] f\_m(s, a)$$
{% endraw %}

where \\( \text{target} = r + \gamma \text{max}_a Q(s', a') \\), and prediction is \\( Q(s, a) \\)

If hypothetically, someone  have the real Q value, i.e target, an optimization like running Q-learning is same as linear regression with the updates.

This is Stochastic gradient descent: we look at one sample at the time. 

However in Q learning, we don't have the target or label.


We want policies \\(\pi \\) that maximize rewards, and not the values that predict them.






## Simplest Policy Search

* start with an initial linear value function or Q function

* Nudge each feature weight up and down and see if your policy is better than before.


## Stochastic Policy
A stochastic policy is a function \\( \pi \\) that maps from the state-action pair to a real value where \\(\pi(s, a) \\) is the probability of taking action \\( a \\) in state \\( s \\)





