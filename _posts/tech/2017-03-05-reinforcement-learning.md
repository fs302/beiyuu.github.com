---
layout: post
title: 深度强化学习系列笔记：强化学习
category: tech 
description: 本文介绍了强化学习的理论框架和模型方法。

---

2016年以来，随着AlphaGo的声名鹊起。深度学习和强化学习在学术界和工业界掀起了波澜大风，在人们探索通用人工智能的路上，他们就像黑白双煞，一个负责特征理解，一个负责找训练粮食。从去年12月起，我们团队也投入大量精力研究和学习DRL领域的技术和算法，并尝试用在电商推荐和智能投放领域。经过3个月的学习，基本建立起深度学习和强化学习的框架，我计划用2篇文章来梳理一下学习到的内容，并尝试做一些扩展。

这是第二篇文章，主要梳理强化学习的理论框架和深度强化学习的常用方法，一些具体实例待后续补充。

以下是根据David.Silver的[强化学习课程](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html)和[《Deep Reinfocement Learning Tutorial》](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Resources_files/deep_rl.pdf)整理：

### Basic RL

* The RL Problem : trial-and-error learning
    - Game View
        + Environment
        + Agent/Player  
    - Components
        + Reward: Env -> Agent
        + Action: Agent
        + Observation: Agent
        + State: Env/Agent
    - Exploration & Exploitation
        + Exploration finds more information about the environment
        + Exploitation exploits known information to maximise reward
    - Prediction & Control
        + Prediction: evaluate the future
        + Control: optimize the future
* Major Components of an RL Agent
    - Policy: agent's behavior function, map from state to action
    - Value Function: predictions about the future, Expected future reward
    - Model: agent's representation of the environment 
* Markov Decision Process
    - Intro
        + MDP formally describe an environment for reinforcement learning
        +  Almost all RL problems can be formalized as MDPs.
    - Markov Process $<S, P>$
        + S is a (finite) set of states
        + P is a state transition probability matrix  
    - Markov Reward Process $<S, P, R, \gamma>$
        + R is a (expected) reward function
        + $\gamma$ is a discount factor, $\gamma$ in [0,1]
    - Markov Decision Process $<S, A, P, R, \gamma>$
        + A is a finite set of actions 
    - Bellman Equation
        + recursion for expected rewards 
        + $v = R + \gamma Pv$
        + iterative methods for large MRPs
            - Dynamic Programming
            - Monte-Carlo Evaluation
            - Temporal-Difference Learning 
* Multi-Armed Bandits
    - Regret: opportunity loss for one step
    - Minimizing total regrets
        + epsilon-greedy algorithm
        + decaying epsilon-greedy algorithm
        + Upper Confidence Bound
        + Thompson Sampling

### Prediction & Control

* Model-Related: Dynamic Programming
    - Why Dynamic Programming for MDP?
        + Bellman equation gives recursive decomposition [Overlapping subproblems]
        + Value function stores and reuses solutions [Optimal substructures]
    - Full Backup
    - Policy Iteration: $\pi'(s)=argmax_{a \in A} q^\pi(s,a)$
    - Value Iteration:$V'(s)=max_{a \in A}(R_s^a+\gamma\sum_{s' \in S}P_{ss'}^aV(s'))$
* Model-Free: no knowledge of MDP transitions / rewards
    - Goal: learn $ V^\pi $ online from experience under policy $ \pi $
    - Sample Backup
    - Monte-Carlo Learning
        + No *bootstrapping*（不做局部采样）
        + All episodes must terminate（所有采样必须到终态）
        + Update value $V(S_t)$ towards actual return $G_t$
        + $V(S_t)=V(S_t)+\alpha(G_t-V(S_t))$
        + high variance, zero bias
    - Temporal-Difference Learning
        + learns from incomplete episodes, by *bootstrapping* （局部采样）
        + updates value $V(S_t)$ towards *estimated* return $R_{t+1}+\gamma V(S_{t+1})$
        + $V(S_t)=V(S_t)+\alpha(R_{t+1}+\gamma V(S_{t+1})-V(S_t))$
        + low variance, some bias
        + more efficient

### Approaches to Reinforcement Learning

* Value-based Deep RL
    - Estimate the optimal value function Q*(s,a)
    - Deep Q-Networks, $D(s,a,w) \approx Q*(s,a)$
    - Improvements
        + Double DQN 
        + Experience Replay
        + Prioritized replay
        + Dueling network
* Policy-based Deep RL
    - Search directly for the optimal policy $\pi*$
    - Actor-Critic Algorithm
    - Deep DPG
* Model-based Deep RL
    - Build a model of the environment 
