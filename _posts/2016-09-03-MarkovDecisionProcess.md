---
layout: post
title: Markov Decision Process
tags: Markov_Decision_Process value_function Reinforcement_learning
category: ml
summary: 
img_post: 20160903-decision-process.png
github-link: na
---

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

I was introduced to Reinforcement Learning (RL) via the [Machine Learning Nanodegree of Udacity](https://www.udacity.com/course/machine-learning-engineer-nanodegree--nd009), which includes a dedicated module on the topic. The main RL method covered was **Q-learning** (you can read my course notes [here - soon to come Part1]().


Reinforcement learning is an area of machine learning, for which An agent (for exampl a car, a robot, etc...) is taucht to take actions in an environment as to maximize some notion of cumulative reward. bounded rationality.

The environment is typically formulated as a Markov decision process (MDP) as many reinforcement learning algorithms for this context utilize dynamic programming techniques.

Reinforcement learning differs from standard supervised learning in that correct input/output pairs are never presented, nor sub-optimal actions explicitly corrected. Further, there is a focus on on-line performance, which involves finding a balance between exploration (of uncharted territory) and exploitation (of current knowledge). The exploration vs. exploitation trade-off in reinforcement learning has been most thoroughly studied through the multi-armed bandit problem and in finite MDPs.

"Reinforcement learning (RL) is learning by interacting with an environment. An RL agent learns from the consequences of its actions, rather than from being explicitly taught and it selects its actions on basis of its past experiences (exploitation) and also by new choices (exploration), which is essentially trial and error learning. The reinforcement signal that the RL-agent receives is a numerical reward, which encodes the success of an action's outcome, and the agent seeks to learn to select actions that maximize the accumulated reward over time. (The use of the term reward is used here in a neutral fashion and does not imply any pleasure, hedonic impact or other psychological interpretations.)"


A2A. Reinforcement learning is the intersection of machine learning, decisions & control, and behavioral psychology. The intersection can be approached from all the three sides, and a detailed explanation is beyond the scope of a quora answer. That said, I'll try to give a short account of all the three perspectives.

Machine Learning: From the ML perspective, RL is the paradigm of learning to control. Think about how you learnt to cycle or how you learnt to play a sport. These learning tasks are not supervised - no one tells you the correct move to make in a board position, or exactly the amount of angle to lean sideways to balance the cycle. They are also not completely unsupervised since some feedback is observed - whether you won or lost the game after a sequence of moves, how frequently do you fall from cycle. Thus, RL is learning to make good decisions from partial evaluative feedback.
Control & Decision theory: In control theory (and AI planning), perfect knowledge about the world is assumed, and the objective is to find the best way to behave. However, for many problems knowledge about the world is not perfect. Hence, exploring the world could increase our knowledge and eventually help us make better decisions. RL is balancing the exploration-exploitation trade-off in sequential decision making problems.
Behavioral Psychology: The simplified goal of behavioral psychology is to explain why, when, and how humans make decisions. We consider humans as rational agents, and hence psychology is also to some extent trying to explain rational behavior. One can study the biological principles of how opinions are formed, which have close connections to temporal difference learning and eligibility traces. RL is the paradigm to explain how humans form opinions and learn to make good decisions with experience.


In Reinforcement Learning:

1. there is no Supervisor, only Reward Signal.

2. Feedback is delayed, not instantaneous

3. Time matters (sequential, non iid data)

4. Agent's actions affect the subsequent data it receives

<svg width="400" height="400" xmlns="http://www.w3.org/2000/svg">
  <path d="M10 10"/>
  <!-- Points -->
  <circle cx="120" cy="120" r="100" stroke="black" fill="#044B94" fill-opacity="0.4"/>
    <text x="30" y="100" font-family="Verdana" font-size="20"> Planning </text>
  <circle cx="220" cy="120" r="100" stroke="black" fill="#ffffcc" fill-opacity="0.4"/>
  <text x="230" y="120" font-family="Verdana" font-size="20"> uncertainty </text>
    <circle cx="170" cy="220" r="100" stroke="black" fill="#ff99ff" fill-opacity="0.4"/>
    <text x="130" y="280" font-family="Verdana" font-size="20"> Learning </text>
	<text x="140" y="80" font-family="Verdana" font-size="22"> MDP </text>
    <text x="130" y="110" font-family="Verdana" font-size="22"> POMDP </text>
    <text x="150" y="170" font-family="Verdana" font-size="22"> RL </text>
</svg>

<div style="align:center; width:100%">
<table style="width:750px;align:center">
	<tr>
		<td style="width:250px"></td>
		<td style="width:300px">Deterministic</td>
		<td style="width:200px">Stochastic</td>
	</tr>
	<tr>
		<td>Fully observable</td>
		<td>A<sup>*</sup>, Depth first, Breath first</td>
		<td>MDP</td>
	</tr>
	<tr>
		<td>Partially observable</td>
		<td></td>
		<td>POMDP</td>
	</tr>
</table>
</div>

## Markov Decision Process, planning under uncertainty

State Transition graph

------- 
graph here
-------



This becomes *Markov* if the outcome of actions are somewhat random. For example at $$ s_1 $$, $$ a_1 $$ results to $$ s_2 $$ with 50% probability and $$ s_3 $$ with 50% probability.

### Parameters of an MDP

1. **states**: $$( s_1, s_2,........., s_N $$

$$
 P(s_{t+1}| s_t) = P(s_{t+1}|s_1, s_2, ....s_t ) 
 $$

i.e the effect of an action taken in a state depend only on that state and not on prior history.
A possible *Markov state* for the exemle of the Helicopter might include: current position, velocity, angular velocity, wind direction. From there, the agent has enough information to decide on next action without having to know previous states.

**MDP** also assumes that Policies are stationary, i.e: 

if  $$ [s_1, s_2, ...s_N] $$  is better than $$[p_1, p_2, ......, p_N] $$

then $$ [ k, s_1, s_2, ...s_N] $$  will be better than $$[k, p_1, p_2, ......, p_N] $$


2. **action**: $$ a_1, a_2, ......, a_K $$


3. **Transition function** or **state Transition Matrix** (aka Model, Dynamics)

$$ 
T(a, s, s' ) = P(s' | a, s) 
$$

It gives the probability that action *a* at state *s* leads to state *s'*:

* Deterministic actions: 
$$ P(s' | a, s) = 1 $$

* Stochastic actions: $$ P(s' | a, s) < 1 $$.
The agent has a probability to move to a different state than what the action intended.

*P* can be represented by a tensor such that $$P_{ijk} $$ is the probability to of going from state $$i $$ to state $$k$$, given action $$j$$.


4. **Reward** function $$ R $$:

* $$ R(a, s, s') $$ reward is attached to $$ a $$, $$ s$$ and $$ s'$$
* $$ R(a, s) $$ reward is attached to $$ a, s$$
* $$ R(s) $$ reward is attached to $$ s $$


### Some definitions

* Policy

The **Policy** is a map from state to action:
- Deterministic:
$$ \pi (a) = a $$
- stochastic: 
$$ \pi(a|s) = P(A=a|S=s) $$

The probability of taking action $$ a $$ conditionned on being on state $$ s $$ (this is useful for exploratory behavior).

<table style="width:500px; background-color: #FFFFFF;">
	<tr style="height:50px; background-color: #FFFFFF;">
		<td style="background-color: #000000; width:100px;"></td>
		<td style="width:100px"></td>
		<td style="width:100px"></td>
		<td style="width:100px"></td>
		<td style="width:100px">goal</td>
	</tr>
	<tr style="height:50px;background-color: #FFFFFF;">
		<td>.</td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
	<tr style="height:50px;background-color: #FFFFFF">
		<td>start</td>
		<td></td>
		<td></td>
		<td  style="background-color: #000000;"></td>
		<td></td>
	</tr>
	<tr style="height:50px; background-color: #FFFFFF">
		<td>.</td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
</table>

The arrows represent policy $$ \pi(s) $$ for each state $$ s $$.

A **Policy** tells what action to take in a particular state: it does not give the sequence of actions to take from a particular state to the 'absorbing' state: this is called a **plan**.
The optimal policy $$ \pi^* $$ is the policy with the highest expected state value function or utility.

### Objective of a MDP
The objective of the **MDP** is to maximize the expected sum of all future rewards i.e 
$$  \textbf{E} \left[ \sum_{t=0} ^{\infty} \gamma^t R_t \right] $$, where $$  \gamma $$ is the discount factor $$  0 \leq \gamma \leq 1 $$.


- $$  \gamma \rightarrow 0 $$ myopic evaluation (agent values immediate reward)
- $$  \gamma \rightarrow 1 $$ far sighted evaluatio (agent values delayed reward)

With discounted reward, the expected sum of all future rewards is bounded. We can show that:

$$ \textbf{E} \left[ \sum_{t=0} ^{\infty} \gamma^t R_t \right] \leq \frac{1}{1 - \gamma} R_{\text{max}} $$ 

where $$  R_{\text{max}} $$ is the maximum reward value (for example 100 in the absorbing state).


### Value State-function 

The value state-function is the **expected discounted future reward** (return), starting from state $$  s $$ and execute policy $$  \pi $$:


$$ \text{V}^{\pi}(s) = \textbf{E}_{\pi} \left[ \sum_{t=0} ^{\infty} \gamma^t R_t | s_0 = s\right] $$


It is used to evaluate how good is a state.


The value function can therefore be decomposed into 2 parts:

- immediate rewards $$ R_t $$
- discounted value of successor state

$$ \begin{split} \text{V}^{\pi} (s_t) &= \textbf{E}_{\pi} \left[ R_t + \gamma R_{t+1} + \gamma^2 R_{t+2} +..........\right] \\ &= \textbf{E}_{\pi} \left[ R_t + \gamma \times(R_{t+1} + \gamma R_{t+2} +..........) \right] \\ &= \textbf{E}_{\pi} \left[ R_t + \gamma \textbf{V}^{\pi}(s_{t+1}) | S = s_t\right] \end{split} $$

It 	is important to remember that $$ R_t $$ is the rewar entering a state, and is not the same as the value state function or utlity for that state $$ R(s)$$ is the immediate feedback that we get when entering that state. In contrast $$  \text{V}(s) $$ gives long term feedback i.e the reward we get from that state and then reward we will get from that point. 

The expected value or predicted value of a variable is the sum of all possible values each multiplied by the probability of its occurence. Hence:

$$ \text{V} (s_t) = R_t + \sum_{s' \in S} P_{ss'} \text{V}(s') $$

We can then recursively calculate the value function that converge towards the optimal value function.

#### Value Iteration Algorithm : a method to calculate the value function

* Pseudo code:

<table>
	<tr>
		<td></td>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
	</tr>
	<tr>
		<td>a</td>
		<td></td>
		<td></td>
		<td></td>
		<td>+100</td>
	</tr>
	<tr>
		<td>b</td>
		<td>0</td>
		<td style="background-color:#000000;"></td>
		<td>0</td>
		<td>-100</td>
	</tr>
	<tr>
		<td>c</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
	</tr>
</table>

1. set $$ \text{V}(s) = 0 $$ for all state except for the Terminal/Absorbing States 

2. Update $$ \text{V}(s) $$ using Backup Equation:

$$\text{V}(s) \leftarrow \left[ \text{max}_a \gamma  \sum P(s'|s,a) \text{V}(s') \right] + R(s) $$

	2.1 Repeat backup equation until convergence


At convergence:

$$\text{V}(s) = \left[ \text{max}_a \gamma  \sum P(s'|s,a) \text{V}(s') \right] + R(s)$$

(the **Bellman Equation** )

+ Exemple 1: assume deterministic $$ P(s'| s,a) = 1 $$ and $$ R_s = -3 $$

<table>
	<tr>
		<td>$$ V(a3) = \text{East} $$</td>
		<td> East</td>
		<td>$$ 100 \times 1 - 3 $$</td>
		<td>$$ =97 $$</td>
	</tr>
	<tr>
		<td></td>
		<td>West</td>
		<td>$$ 0 \times 1 - 3 $$</td>
		<td>-3</td>
	</tr>
	<tr>
		<td></td>
		<td>North</td>
		<td>$$ 0 \times 1 - 3 $$</td>
		<td>-3</td>
	</tr>
	<tr>
		<td></td>
		<td>South</td>
		<td>$$ 0 \times 1 - 3 $$</td>
		<td>-3</td>
	</tr>
</table>

The maximum $$ V $$ at state $$ a3 $$ is $$ \text{max}_a = 97 $$



+ Exemple 2: assume Stochastic behavior i.e $$ P=0.8 $$ is the probability that the action perfomed at $$s$$ results in the agent being in the intended state $$ s' $$. There is 10% chance that the agent moves in direction that are $$ 90^o $$ off the intended direction. 

<table>
	<tr>
		<td>$$ V(a3) = \text{East} $$</td>
		<td> East</td>
		<td>$$ 0.8 \times 100 + 0.1 \times 0 +  0.1 \times 0 - 3 $$</td>
		<td>$$ =77 $$</td>
	</tr>
	<tr>
		<td></td>
		<td>West</td>
		<td>$$ 0.8 \times 0 + 0.1 \times 0 + 0.1 \times 0- 3 $$</td>
		<td>-3</td>
	</tr>
	<tr>
		<td></td>
		<td>North</td>
		<td>$$ 0.8 \times 0 + 0.1 \times 0 + 0.1 \times 100 - 3 $$</td>
		<td>-3</td>
	</tr>
	<tr>
		<td></td>
		<td>South</td>
		<td>$$ 0.8 \times 0 + 0.1 \times 0.1 + 0.1 \times 100 - 3 $$</td>
		<td>-3</td>
	</tr>
</table>


The maximum $$ V $$ at state $$ a3 $$ is  $$ \text{max}_a = 77 $$

<table>
	<tr>
		<td>$$ V(a3) = \text{East} $$</td>
		<td> East</td>
		<td>$$0.8 \times -100 + 0.1 \times 0 +  0.1 \times 0 - 3 $$</td>
		<td>$$ < 0 $$</td>
	</tr>
	<tr>
		<td></td>
		<td>West</td>
		<td>$$ 0.8 \times 0 + 0.1 \times 77 + 0.1 \times 0- 3 $$</td>
		<td>= 4.7 </td>
	</tr>
	<tr>
		<td></td>
		<td>North</td>
		<td>$$ 0.8 \times 77 + 0.1 \times -100 + 0.1 \times 0 - 3 $$</td>
		<td>= 48.6</td>
	</tr>
	<tr>
		<td></td>
		<td>South</td>
		<td>$$ 0.8 \times 0 -100 \times 0.1 + 0.1 \times 0 - 3 $$</td>
		<td>< 0</td>
	</tr>
</table>


The maximum $$ V $$ at state $$ b3 $$ is $$ \text{max}_a = 48.6 $$



## Optimal Policy

The optimal policy is the policy that maximizes the long term expected rewards and the best action is that that maximizes $$ \text{V}(s) $$:

$$ pi^* (s) = \text{argmax}_a \sum_{s'} P(s'|s,a) \text{V(s') $$

Note that $$ argmax_a $$ returns an action.


Exemple1: assuming $$ \gamma = 1 $$ and $$ r=-3 $$
State value function after convergence
<table>
	<tr>
		<td>85</td>
		<td>89</td>
		<td>93</td>
		<td>+100</td>
	</tr>
	<tr>
		<td>81</td>
		<td style="background-color:#000000; "></td>
		<td>68</td>
		<td>-100</td>
	</tr>
	<tr>
		<td>77</td>
		<td>73</td>
		<td>70</td>
		<td>47</td>
	</tr>
</table>

$$\pi^*$$
<table>
	<tr>
		<td>right</td>
		<td>right</td>
		<td>right</td>
		<td>+100</td>
	</tr>
	<tr>
		<td>up</td>
		<td style="background-color:#000000; "></td>
		<td>up</td>
		<td>-100</td>
	</tr>
	<tr>
		<td>up</td>
		<td>left</td>
		<td>left</td>
		<td>left</td>
	</tr>
</table>


## action value function (or *q* values)

The action value function $$ q_{\pi}(a,s) $$ is the expected return starting from state $$ s $$, taking action $$a$$, and then following policy $$ \pi $$:

$$ q_{\pi} (s, a) = \textbf{E} \left[ R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} ....| S_t=s, A_t=a \right]$$

The action-value function can be decomposed into immediate rewards and discounted reward of successor state:

$$ q_{\pi}(s, a) = \textbf{E}_{\pi} \left[ R_{t+1} + \gamma q_{\pi}(S_{t+1}, A_{t+1}) | S_t =s, A_t =a\right]$$

$$ q_{\pi}(s, a) = R(s,a) + \gamma \sum _{s' \in S} P(s,a,s') \text{V}_{\pi}(s')$$


Note that if  $$ R(s, a, s')$$ i.e the immediate reward depends on the triplet $$ (s, a, s')  $$:

$$ q^* (s, a) = \sum P(s'| a, s) \left[ R(s, a, s') + \gamma \text{V}(s') \right] $$

where $$ \gamma \text{V}(s') $$ is the discounted future reward.

$$ q^* (s, a) = \sum P(s'| a, s) \left[ R(s, a, s') + \gamma \text{max}_{a'} q(s', a') \right] $$



Bellman Optimality Equation:


1. Bellman optimality equation for $$ q^{\*} $$:

$$ q_{\pi} (s, a) = R(s, a) + \gamma \sum_{s' \in S} P_{s, a, s'} \text{max}_{a'} q_{\pi} (s', a') $$


where $$ v^{\*}(s) = \text{max}_{a'} q^*(s, a) $$ at state $$ s $$, action with maximum $$ q $$



### $$ q $$ value iteration

Pseudo-code:

1. initialize all $$ q(s,a) $$ to 0

2. Repeat until convergence:

for all $$ q $$-states, s, a
	- compute $$ q_{t+1}(s,a) $$ from $$ q_t $$ by Bellman backup at $$ (s, a) $$
	- until $$ \text{max}_{s, a} |q_{t+1}(s, a) - q_t(s,a)| < \epsilon $$
$$ q_{t+1} (s, a) \leftarrow R(s, a) + \sum P(s, a, s') \text{v}_{\pi}(s')$$


Exemple: what's the q-value of going down from state $$ a_2 $$

<table>
	<tr>
		<td></td>
		<td></td>
		<td>$$a_2$$</td>
		<td></td>
	</tr>
	<tr>
		<td>81</td>
		<td> right: 0.868</td>
		<td>up: 0.881| dwn:0.673 | left:0.812 | right: 0.918</td>
		<td>+1</td>
	</tr>
	<tr>
		<td></td>
		<td></td>
		<td>up:0.660 | left: 0.641 | down: 0.415 | right: -0.60</td>
		<td>-1</td>
	</tr>
</table>

$$(0.8 \times 0.660 + 0.1 \times 1 + 0.1 \times 0.868) = 0.675$$



1. Bellman optimality equation for $$ v^{\*} $$:

$$ v^{\*}(s) = \text{max} _a q^{\*} (s, a) $$


The optimum policy performs the optimal action value function:

$$ \pi ^{\*} (s) = \text{argmax} _a q(s, a) $$

## Optimal policy

$$ \pi $$ is a better policy than $$ \pi' $$ if $$ v_{\pi}(s) \geq v_{\pi'} (s) $$ in all state.

For any **MDP**, there exists an optimal policy $$ \pi^{\*} $$ that is better or equal to all other policies: $$ \pi^{\*} \geq \pi $$ forall \(( \pi $$

There can be more than one optimal.


All optimum policies achieve the optimal state value function:

$$ v _{ \pi ^{\*} } (s) = v^{\*} (s) $$




## Q-learning 
**Q-learning** answers the question of how an agent should behave in an MDP when it does not know the Transition probabilities (and the reward function), i.e the agent just takes actions and receives reward.

We can use the ** asynchronous Value Iteration **.

*Q*-learning uses temporel differences to update its estimate of the value of $$ Q(s,a) $$. In *Q*-learning, the agent maintains a table of $$ Q(s,a) $$ value for each pair of state-action.

An experience < $$s, a, r, s' $$ > provides one data point for the value of $$ Q(s,a) $$.

$$ Q $$-learning is an off-policy algorithm for Temporel Difference Learning.

Q-learning learns the optimal policy even when actions are selected according to a more exploratory or even random policy.


** Q-learning** algorithm:
1. Choose an action:
- choose the action that appears to maximize $$ Q $$ (based on current estimates)
- choose a random action the rest of the time
- reduce the chance of taking a random action at time goes on.

2. Update the **Q-function**:
- let $$ \alpha $$ be a learning rate ( $$ 0 \leq \alpha \leq 1 $$ )
- let $$ \gamma $$ be discount factor
- whenever the agent starts out in state $$ s $$, take action $$ a $$ and end up in state $$ s' $$.
the update rule of $$ Q(s, a) $$:

$$ Q(s, a) \leftarrow (1- \alpha) Q(s, a) +\alpha(R(s, a, s') + \gamma \text(max)_{a'}Q(s', a'))) $$

3. under reasonable conditions, $$Q $$-learning converges towards the true $$ Q $$-value , eventhough it never learns transition probabilities.

Why can we get away with that: because the frequency of observing each $$ s' $$ already depends on them. Thus, we say $$ Q $$-learning is **model-free**.


Pseudo-code:

For each $$ (s,a) $$ state-action pair 

    1. initialize to zero or non-zero values $$ q(s,a)=0 $$ 

    2. observe correct state $$s$$

    3.Do:
    	- select an action $$a$$ and execute it
    	- receive immediate reward
    	- observe new state $$s'$$
    	- update the table entry for $$q(s,a)$$ as follows:

    	$$q(s, a) = r + \gamma_{a'} q(s', a') $$
    	- $$ s = s' $$

   Convergence: our approximation will converge towards the true $$ q $$, but we must vist every state-action pair infinitely many times.

------
Q learning convergence:

Q starts anywhere
Q(s,a) <--- (\alpha_t)      r + \gamma max_a' q(s', a')

then q(s,a) <---- q(s, a)

if s, a visited infinitey often:

\sum_t \alpha_t = \inf

\sum_t \alpha_t ^2 < \inf

s' \approx T(s, a, s')

r \approx R(s)


### Temporal difference

$$ v^*(s) = \text{max}_a (R_s ^a  + \gamma \sum_{s' \in S} P_{s, s'} ^a v^* (s') q^*(s,a) =  R_s ^a + \gamma \sum_{s' \in S} P_{s, s'} \max_a q^*(s',a)$$

Bellman Optimality Equation is non-linear, hence the need to use Iterative Solution Mothods:

1. Value Iteration

2. Policy Iteration

3. Q-learning

4. Sarsa

1. Init $$ Q(s,a)=0 $$
2. Update rule for $$Q $$-learning:

\$\$ Q(s_t,a_t) = Q(s_t, a_t) + \alpha \times (r_{t+1} + \gamma \text{max}_a ( Q( s _{t+1}, a_t) - Q(s _t, a_t) ) \$\$


where  $$ r_{t+1} $$ is the reward perceived after action $$a_t$$ is performed (immediate reward). $$ \alpha $$ is  the learning rate  $$  0 < \alpha < 1 $$, and quantifies how much we update the $$ Q $$-values).
$$ \epsilon $$ controls the trade-off between exploration and exploitation. $$ \epsilon = 56 \% $$ means that the agent will do random action half of the time.



## How to explore
Several schemes for forcing exploration:

1. simplest: random actions ($$\epsilon$$-greedy)

2. At every step, flip coins

3. with small probability $$ \epsilon $$, act randomly

Howver, the problem with random actions is that you do eventually explore the space but keep transiting around, once learning is done. One solution is to lower $$\epsilon$$ over time.


## Exploration versus exploitation

Reinforcement learning is like a trial and error learning, where the agent should discover a good policy without loosing too much reward along the way:

1. exploration: find more information about the environment
2. exploitation : exploits known information to maximize reward

Example1: 
* for restaurant selection: exploitation= go to favourite restaurant / exploration: try new restaurants(s)
* online Banner ad









This may be of interest to people here but it is not needed to solve the project.

I attended a meetup of some really advanced machine learning people where terms SARSA (and off-policy and on-policy learning) came up. Out of curiosity I looked it up here is what I found on the internet (I am copying it verbatim, it is not my work):

1) Think of the policy just as the question of: "what action (a) to take 
in a state (s)?"
2) Think of the Q-function just as the question of: "how good is it to 
take action (a) in a state (s)?"
Don't mix them (to answer 1, question 2 may be taken into account, but 
needn't necessarily, you can also dice).

Now compare the update formulas, which form the outcome of the answer to 
the second question:

SARSA:
Q(s,a) <- Q(s,a) + alpha*[r + gamma*Q(s',a') - Q(s,a)]

Q-Learning:
$$ Q(s, a) \leftarrow Q(s, a) + \alpha \times \left[ r + \gamma \times \text{max}\_a Q(s', a') - Q(s, a)]

In SARSA, the answer to the first question is taken into account, 
therefore it manipulates the answer to the second question.

In Q-Learning, nothing, that has to do with the first question is 
taken into account, thus, the first question here has no influence to 
the outcome of the correct answer to the second question. You can dice 
for each action to take, the correct answer of 2 stays the same.

