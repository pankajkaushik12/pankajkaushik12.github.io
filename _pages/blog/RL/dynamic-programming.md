---
title: Dynamic Programming
permalink: /blog/RL/dynamic-programming
layout: single
author_profile: false
classes: wide
---


In the context of *Reinforcement learning*, the *Dynamic programming* refers to a collection of algorithms used to compute optimal policies, assuming a perfect model of the environment is available. The search for good policies involves optimizing value functions, such as the state-value function $$v_{*}$$ and the action-value function $$q_{*}$$.
## Policy Evaluation
*Policy evaluation* refers to computing the state-value function $$v_{\pi}$$ for a given policy $$\pi$$ using the following recursive Bellman equation:

$$
v_{\pi}(s) = E_{\pi}[G_t | S_t = s] \\
= E_{\pi}[\sum_{k = 0}^{\infty} \gamma^k \cdot R_{t+k+1} | S_t = s] \\
= E_{\pi}[R_{t+1} + \gamma \cdot v_{\pi}(S_{t+1}) | S_t = s] \\
= \sum_{a} \pi (a|s) * \sum_{s', r} p(s', r | s, a)[r + \gamma * v_{\pi}(s')]    \tag{1}
$$

If the environment's dynamics are known exactly, the above equations form a system of $$∣𝑆∣$$ linear equations in the unknowns $$𝑣(𝑠)$$ for each state $$𝑠 \in 𝑆$$. For practical cases, we use the iterative approach to estimate the value function $$v(s)$$ for each state. 

In the initial stage, we choose an arbitrary value for the value function for each state *s*, and then we update the state value using the *Bellman equation*.

$$
v_{k+1}(s) = E_{\pi}[R_{t+1} + \gamma * v_k(S_{t+1}) | S_t = s] \\
= \sum_{a}^{} \pi (a|s) \sum_{s', r} p(s', r | s, a)[r + \gamma \cdot v_{k}(s')]
$$

At each sequence step *k*, the new value of state is obtained by using the old value of future states of *s*. The iterative process terminates when the maximum change in state values across all states falls below a specified threshold:

$$
max_{s \in S} |v_{k+1}(s)−v_k(s)| < \theta
$$

According to *Bellman equation*, as the k $$\rarr$$ $$\infty$$, $$v_{k}$$ $$\rarr$$ $$v_{\pi}$$. This process of estimating state values is called *iterative policy evaluation*.

## Policy Improvement
After determining the value for each state *s* for a policy $$\pi$$. The next question that comes up in mind is wheather the policy $$\pi$$ is best policy or not. Suppose we have another policy $$\pi'$$ that is exactly same as $$\pi$$ except in one state *s* where policy $$\pi'$$ chooses action *a* $$\neq$$ $$\pi(s)$$

## Policy Iteration


## Value Iteration


## Asynchronous Dynamic Programming


## Generalized Policy Iteration
