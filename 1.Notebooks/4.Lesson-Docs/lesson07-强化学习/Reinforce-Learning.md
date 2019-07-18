# 强化学习

## 强化学习概述

### 强化学习的分类

- 监督学习
- 无监督学习
- 半监督学习
- 与环境的交互中学习

### 什么时候强化学习更有用？

- 第二步效应
- 长期效应

### 理解强化学习的几个基本定义

- 实体(Agent)
- 环境(Environment)
- 状态(State)
- 观察(Observation)
- 行动(Action)
- 回报(G or Reward)

![1563409440761](C:\dev\AI-training\1.Notebooks\4.Lesson-Docs\lesson07-强化学习\image-RL\Basic-Concept.png)

### 强化学习的几个关键概念

- 模型(Model)：对环境/系统的描述
- 策略(Policy)：实体(Agent)的行为/动作
- 评估值(Value)：评估状态(State)/行动(Action)的优劣

### 强化学习的历史与发展

![1563411658562](C:\dev\AI-training\1.Notebooks\4.Lesson-Docs\lesson07-强化学习\image-RL\RL-history.png)

### 几个强化学习的应用案例



## 强化学习基础知识与查找表方法（tabular approaches）

### 马尔可夫链(Markov Chain)

$$
𝑃(𝑠_{𝑡+1}|𝑠_1,…,𝑠_𝑡)=𝑃(𝑠_{𝑡+1}|𝑠_𝑡)
$$

![1563412033683](C:\dev\AI-training\1.Notebooks\4.Lesson-Docs\lesson07-强化学习\image-RL\Markov.png)

### 马尔可夫决策过程（MDP）

$$
r(s,a,s′)=E[R_{t+1}|S_t=s,A_t=a,S_{t+1}=s′]
$$

### Action-Value Function

用行动价值函数(Action-Value Function)来评估一种状态的优劣、进而遵循一项策略来采取一项行动。

### Optimal Value Functions

最佳值函数(Optimal Value Functions)可以获得最优值。

### 连续决策中的两个基本问题

1. Learning
2. Planning(策略评估)

#### 贝尔曼期望等式(Bellman Expectation Equation)

$$
v_𝜋(𝑠)=𝐸_𝜋[𝑟_{𝑡+1}+𝛾𝑟_{𝑡+2}+𝛾^2𝑟_{𝑡+3}+⋯|𝑠]=𝐸𝜋[𝑟_{𝑡+1}+𝛾𝑣_𝜋(𝑠′)|𝑠]
$$

### MDP中的两种强化学习方法

#### 1. Model free learning

- 蒙特卡洛方法(Monte-Carlo Policy Evaluation)
- 时序差分方法(Temporal-Difference Policy Evaluation)

#### 2. Model based learning

- 利用经验数据做预估模型
- 根据预估模型计算最优策略

### 大规模强化学习简介



## 基于值函数（Value）和基于策略（Policy）的算法 

### *Value-based Algorithms*

- 查找表的局限

#### 什么是Q-learning

- Q-Learning : 在一个给定的状态(State)时，采取了一个特定的行动(Action)后，能得到的奖励(Reward)是什么

#### 深度Q-learning网络 (DQN: Deep Q-learning Network)

- 采用神经网络利用Q-learning做函数逼近
- 利用贝尔曼方程做收敛

#### 分布式强化学习(Distributional RL)

- 考虑价值函数的分布
- 分布相比于均值/期望能够对决策提供更多的信息

##### 如何表达价值函数的分布表？

##### 什么时候使用分布式深度学习最有效？

### *Policy-based Algorithms*

- 策略梯度算法
- 策略优化算法

#### 如何利用样本数据计算梯度？

- (Stochastic) Policy Gradient Theorem(1999)
- Deterministic Policy Gradient Theorem(2014)

#### 如何有效的利用数据？

- 关键点：新策略不能太远离原策略

### *总结：怎样选择合适的算法*

![1563424107457](C:\dev\AI-training\1.Notebooks\4.Lesson-Docs\lesson07-强化学习\image-RL\Summary of Algorithm.png)



## 强化学习的典型应用

### 游戏与娱乐

- DQN的应用
- Alpha Star

### 从AlphaGo到Alpha Zero

### 神经生物学(Neuro Science)

### 音乐与影视

### 健康与医疗

### 贸易与物流

### 自然语言处理(NLP)

### 自动驾驶与机器人

### 实际应用中如何定义State\Action\Reward ?

### 实际应用中如何选择Policy\Value\Algorithm ?

- CNN与RNN的适用场景



## 动手实践强化学习 

提供游戏环境，考虑离散性和连续性



## 强化学习的机遇与挑战 

- 多实体游戏的处理
- 强化学习的鲁棒性
- 强化学习的安全性
- 样本的有效性