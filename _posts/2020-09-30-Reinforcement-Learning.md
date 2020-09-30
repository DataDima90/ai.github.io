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

A reward can be in a GO Game: 1 if we winn, -1 if we lose. Sometimes, we get rewards more frequently.

The discount factor discounts future rewards if it is smaller than one. Money earned in the future
often has smaller current value, and we may need it for a purely technical reason to converge the solution better.

```math
G_t = R_{t+1}
```
