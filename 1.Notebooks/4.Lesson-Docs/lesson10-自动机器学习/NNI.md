# 自动机器学习

## 课程目标

- 了解自动机器学习的需求及当前状况
- 了解微软NNI(Neural Network Intelligence)自动机器学习工具

## 自动机器学习的定位

<img src ="..\Images\NNI position.jpg">

## AI建模的挑战

- 模型调参工作复杂，难寻规律
- 缺乏经验丰富的算法工程师，数据科学家
- 手工规划超参搜索效率太低
- 算法繁多，无所适从

## 什么是自动机器学习

- 参数：几十万到几百万个数值，大部分是向量和矩阵，机器学习过程中来调整优化这些参数
- 超参(Hyper-parameter)：需要手工设定，模型不能自动调优的参数。如：循环次数，学习率，神经元数量，神经网络结构等等。
- 自动机器学习(Auto Machine Learning)：用机器学习的方法来学习超参 - 机器学习的机器学习

## 自动机器学习概述

### 研究方向与进展

- 超参调优：TPE，SMAC
- 神经网络架构搜索：Evolution，ENAS，Darts

### 机器学习应用的要求

- 又快又好
- 可扩展

### 目标用户

- 数据科学家
- 研究人员

### 调参算法

- Random
- Grid Search
- TPE，SMAC
- Naive Evolution
- Anneal
- ENAS......

### 提前终止算法

- Median Stop
- Curve Fitting

### 自动机器学习的选择困难症

- 各种不同的软件包和工具，很难比较不同的Auto ML算法
- 支持不同的场景，有不同的API和调优方法，如何选择？

<img src ="..\Images\NNI choice.jpg">

- 对研究工作不友好，不可扩展

  大多数工具都是单机运行版本

  大多数工具只支持特定的硬件和框架

## NNI示例

## NNI结果分析

https://github.com/Microsoft/NNI

