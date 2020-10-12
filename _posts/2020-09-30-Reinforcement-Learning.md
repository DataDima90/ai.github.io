---
layout: post
title: Reinforcement Learning
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Reinforcement Learning]
mathjax: true
summary: Introduction to RL
---


Markov Decision Process (MDP) allows us to model a complex problem in a easy way.
An agent (e.g. human, robot arm, etc.) observes the environment and takes some actions.
Rewards are given out but the may be infrequent and delayed. 

Markov decision process (MDP) is defined by:

- Set of states S
- Set of actions A
- Transition function P(s' | s, a)
- Reward function R(s, a, s')
- Start state
- Discount factor $\gamma
- Horizon H

A **state** in MDP can be represented as raw images or for robotic controls, 
we use sensors to measure the joint anglese, velocity and end-effector pose:

An **action** can be a move in a chess game, tic tac toe or moving a robotic arm or a joystick.

A **reward** can be in a GO Game: 1 if we winn, -1 if we lose. Sometimes, we get rewards more frequently.

The discount factor discounts future rewards if it is smaller than one. Money earned in the future
often has smaller current value, and we may need it for a purely technical reason to converge the solution better.

<img src="https://latex.codecogs.com/svg.latex?\Large&space;G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}" />

We can rollout actions forever or limit the experience to N time steps. This is called the horizon.

The **transition function** predicts the next state after taking action. 
It is called the **model** which plays a major role when we discuss Model-based RL later.

### Policy

In RL, our focus is finding an optimal **policy**. A policy tells us how to act from particular state s at time t.

<p align="justify">
<img src="https://latex.codecogs.com/svg.latex?\Large&space;a_t = \pi_{\theta} (s_t)"/>
</p>

Like the weights in Deep Learning methods, this policy can be parameterized by Theta.

We want to find a policy that makes the most rewarding decisions:

<img src="https://latex.codecogs.com/svg.latex?\Large&space;\max_{\theta} E_{\tau \sim p_{\theta} (\tau)} [\sum_t r(s_t, a_t)]"/>

First expression: Find the policy with max rewards
Second expression: expected rewards

Our policy can be deterministic or stochastic. For stochastic, the policy outputs a probability distribution instead.

<img src="https://latex.codecogs.com/svg.latex?\Large&space;p(a_t | s_t) = \pi_{\theta} (a_t | s_t)"/>

In RL, we want to find a sequence of actions that maximize expected rewards.


But there are many ways to solve the problem. For example, we can

- Analyze how good to reach a certain state or take a specific action (i.e. Value-learning),
- Use the model to find actions that have the maximum rewards (model-based learning), or
- Derive a policy directly to maximize rewards (policy gradient).


