---
title: Finite Markov Decision Property
permalink: /blog/RL/finite-markov_decision_property
layout: single
author_profile: false
use_math: true
---

## Key Definition:
- *Agent*: The decision maker that interacts with the environment.
- *Environment*: Everything the agent interacts with.
- *State*: The information about the environment available to the agent.

## Flow
At each time step, the environment presents a state $$S_t$$ to the agent, based on which the agent takes an action $$A_t$$. As a consequence, the agent receives a reward $$R_{t+1}$$, which leads the agent to the next state $$S_{t+1}$$. The agent chooses an action based on the policy $$\pi$$, where $$\pi_t(a \| s)$$ represents the probability of choosing action $$A_t = a$$ given that the agent is in state $$S_t = s$$.

## Goal of the agent
Goal of the agent is to maximize the reward over ther long term. Suppose if the agent received awards as $$R_{t+1}$$, $$R_{t+2}$$, _, _, _, $$R_{T}$$ after time step $$t$$, then it seeks to maximize the return $$G_t$$, 

$$
G_t = \sum_{k=0}^{T} R_{t + k + 1} \tag{1}
$$

where T is the final time step or where the interaction between the agent and environment  breaks. We also call this the end of the episode or trial. The tasks which have such terminal state are called *episodic tasks*, and the tasks where the interaction does not break into episodes are called *continuing tasks*. For continuing tasks as T $$\rightarrow$$ $$\infty$$, the returned sum in equation (1) reaches infinity.

To make the return finite, we consider *discounting*. According to which we maximize the return as

$$
G_t = \sum_{k = 0}^{\infty} \gamma^{k} * R_{t + k + 1} \tag{2}
$$

where $$\gamma$$ is called the *discount rate* and lies between (0, 1).

The *discount rate* determines the current value of the future reward. Suppose, the agent will receive the reward $$R_{t+k}$$ at $$k$$ steps in future, then its value decreases by $$\gamma^{k - 1}$$ times to what it worth if it received immediately.

If $$\gamma$$ = 0, then we called the agent a **myopic** as it only concerned or immediate reward. As the value of $$\gamma$$ increases, the agent becomes more farsighted.

We can define the return of total reward for *episodic task* and *continuing task* tasks with a single notation by adding an *absorbing state* to the terminal state which inturn return to itself by giving reward 0.

$$
G_t = \sum_{k = 0}^{T - t - 1} \gamma ^k * R_{t + k + 1} \tag{3}
$$

that includes the possibility that T = $$\infty$$ or $$\gamma$$ = 1 (Not at same time). If T is finite then to make the sum finite $$\gamma$$ must be less then 1.

## Markov Porperty
The state of the environment at $$t+1$$ depends only the state and action at time $$t$$.

$$
p(s', r | s, a) = Pr[R_{t+1} = r, S_{t+1} = s' | S_t, A_t] \tag{4}
$$

defines the probability of getting a reward *r* by going to state *s'* after taking action *a* in state *s*. $$S_t$$ and $$A_t$$ are the state of the environment and action taken by the agent at time step t. $$R_{t+1}$$ is the reward based on the action $$A_t$$ in state $$S_t$$. $$S_{t+1}$$ is the environment state at $$t+1$$.

A reinforcement learning tasks that satisfies the *Markov property* is called *Markov Decision Process* and its states and actions are finite, then it is called a **finite MDP**.

Using equation (4), we can derive the following:
- *Expected reward for a state-action pair*

    The expected reward the agent may get by taking action *a* in state *s* at time *t* , can be calculated by
    $$
    r(s, a) = E[R_{t+1} | S_t = s, A_t = a] \\
    = \sum_{r\in R}^{} r * \sum_{s'\in S}^{} p(s', r| s, a) \tag{5}
    $$
    - The expectation is taken over all possible next state and reward, given that the agent is in state *s* and takes action *t*.
    - Since rewards are stochastic, we sum over all the possible value of r, each of which is weighted by its probability.
    - p(s', r\| s, a) represents the probability of reaching state *s'* and receiving the award *r*, given that the agent is in state *s* and takes action *t*. This account the fact the agent may get different reward depending on the next state.

- *State transition probability*

    Probability to be in state *s'* by taking action *a* at state *s* is defined as
    $$
    p(s'| s, a) = Pr[S_{t+1} = s' | S_t = s, A_t = a]   \\
    = \sum_{r\in R}^{} p(s', r| s, a)                   \tag{6}
    $$

- *Expected reward for state-action-nextstate*

    $$
    r(s, a, s') = E[R_{t+1} | S_t = s, A_t = a, S_{t+1} = s']   \\
    = \frac{\sum_{r\in R}^{} r * p(s', r| s, a)}{p(s'|s, a)}     \tag{7}
    $$

## Value function

Value function estimates how good it is for the agent to be in a given state. Goodness is defined in terms of expected future reward, which is turn depends on the agent's policy $$\pi$$(a\|s), probability of taking action *a* in state *s*.

We define the expected return given the current state *s* and policy $$\pi$$, as

$$
v_{\pi}(s) = E_{\pi}[G_t | S_t = s] \\ 
= E_{\pi}[\sum_{k = 0}^{\infty} \gamma^k * R_{t+k+1} | S_t = s]        \tag{8}
$$
where the $$G_t$$ is the function of future reward sequence. $$v_{\pi}$$ is called the *state-value function* for policy $$\pi$$.

Similarly, we can define the value of taking an action *a*, given the state *s* and policy $$\pi$$, as

$$
q_{\pi}(s, a) = E_{\pi}[G_t | S_t = s, A_t = a] \\
= E_{\pi}[\sum_{k = 0}^{\infty} \gamma^k * R_{t+k+1} | S_t = s, A_t = a]     \tag{9} 
$$

$$q_{\pi}$$ is called the *action-value function* for policy $$\pi$$.

## Recursive nature of *State-Value function*
For any policy $$\pi$$ and any state *s*, the following consistency condition holds between the value of s and the value of its possible successor states:

$$
v_{\pi}(s) = E_{\pi}[G_t | S_t = s] \\
= E_{\pi}[\sum_{k = 0}^{\infty} \gamma^k * R_{t+k+1} | S_t = s] \\
= E_{\pi}[R_{t + 1} + \gamma * \sum_{k = 1}^{\infty} \gamma^k * R_{t+k+1} | S_t = s] \\
= E_{\pi}[R_{t+1} + \gamma * \sum_{k = 0}^{\infty} \gamma ^ k * R_{t+k+2}|S_t = s] \\
= \sum_{a} \pi (a|s) * \sum_{s'} \sum_{r} p(s', r | s, a)[r + \gamma * E[ \sum_{k = 0}^{\infty} \gamma ^ k * R_{t+k+2} | S_t = s]] \\
= \sum_{a} \pi (a|s) * \sum_{s', r} p(s', r | s, a)[r + \gamma * v_{\pi}(s')]       \tag{10}
$$

If environment state changes to *s'* by receiving award *r* after taking action *a* in state *s*, then expected future reward will be [r + $$v_{\pi}$$(s')]. Here this value is weighted by the p(s', r \| s , a). The sum over all possible pair of *s'* and *r* is then multiplied by the probability $$\pi$$(a\|s), of taking action *a* in state *s*.


## Optimal Value functions
A policy $$\pi$$ is defined to be better than or equal to $$\pi'$$, if its expected return is greater than or equal to expected return of $$\pi'$$ for all states *s*, i.e,

$$
\pi \geq \pi' 
$$
if and only if $$v_{\pi}$$(s) $$\geq$$ $$v_{\pi'}$$(s), $$\forall$$ s $$\in$$ S.

If a policy is better than all other policies, then that policy is called *Optimal policy*, defined as

$$
v_{*}(s) = \max_{\pi} v_{\pi}(s)  \tag{11}
$$

$$\forall$$ s $$\in$$ S.

Optimal policies shares the same optimal *action-value* function. It return the expected return for taking action *a* in state *s*.

$$
q_{*}(s, a) = \max_{\pi} q_{\pi}(s_t, a) \\ 
q_{*}(s, a) = E[R_{t+1} + \gamma v_{*}(S_{t+1}) | S_t = s, A_t = a]
$$

## Bellman Optimality equation

According to this, the value of state under optimal condition $$v_{*}$$(s) equals to the expected return for the best action from that state.

- Bellman equation for $$v_{*}$$
    $$
    v_{*}(s) = \max_{a\in A(s)} q_{\pi _{*}}(s, a) \\
    = \max_{a} E_{\pi _{*}}[G_t | S_t = s, A_t = a] \\ 
    = \max_{a} E_{\pi _{*}}[R_{t+1} + \gamma \sum_{k=0}^{\infty}\gamma ^ k * R_{t+k+2} | S_t= s, A_t = a] \\
    = \max_{a} E[R_{t+1} + \gamma v_{*}(S_{t+1}) | S_t = s, A_t = a] \\ 
    = \max_{a\in A(s)} \sum_{s',r} p(s', r| s, a)[r + \gamma * v_{*}(s')]       \tag{12}
    $$

-Bellman equation for $$q_{*}$$
    $$
    q_{*}(s, a) = E[R_{t+1} + \gamma \max_{a'} q_{*}(S_{t+1}, a) | S_t = s, A_t = a] \\ 
    = \sum_{s',r} p(s', r| s, a)[r + \gamma * \max_{a'} q_{*}(s', a')]          \tag{13}
    $$



## References
1. [Reinforcement Learning: An Introduction, by Andrew Barto and Richard S. Sutton](https://web.stanford.edu/class/psych209/Readings/SuttonBartoIPRLBook2ndEd.pdf). 
