# 自然语言处理(NLP)

## 自然语言处理导论

> 自然语言处理(Natural Language Processing)是人工智能皇冠上的明珠 -- 比尔.盖茨

### 一、发展历史

##### NLP的历史沿革 -- 六个发展阶段

1. 计算机发明，智能理论构建
2. 形式化规则系统
3. 基于HMM的语音识别和篇章建模
4. 知识库构建
5. 统计建模与机器学习
6. 大数据与深度学习

### 二、主要技术

数据，算法，计算能力构成人工智能的三要素

##### 典型应用介绍

1. 智能词典/机器翻译
2. 搜索/机器阅读
3. 聊天机器人 -- 小冰
4. 计算机的创造力 -- 吟诗、作画、谱曲、对联等

##### 最新技术简介

1. 编码-解码机制
2. 注意力(Attention)模型
3. Transformer引入自编码
4. 预训练模型带来新的进步
   - ELMo(Embeddings from Language Models)
   - BERT: Bidirectional Encoder Representations Training

### 三、目前面临的主要问题

##### 计算资源的“军备竞赛”

##### 对数据的过度依赖

- 大语料训练依然存在的错误
- 少语料训练算法的建模
- 多轮问题的常识与推理

### 四、未来之路

> 目标：做出可解释、有知识、有道德、可自我学习的NLP系统。

##### 系统有序地推进NLP的创新

6个角度：计算能力、数据、模型、人才的培养、多维度的合作、应用



## 神经网络NLP概要

### 词嵌入

##### 词向量(Word Vector)的概念

神经语言模型：三层神经网络对自然语言建模，并在该过程中获取词向量

##### 面临的问题

- 无法处理未知和OOV(Out Of Vocabulary)词汇
- 无法处理多义词

### 编码-解码(Encoder-Decoder)

递归神经网络RNN(Recurrent Neural Network)

##### 编解码中的RNN与Attention

​	Google: 《Attention is all you need》

##### Transformer的整体模型框架

##### <img src = "..\Images\Transformer.png">

##### 采用RNN和Transformer的区别

### 预训练模型

什么是预训练？为什么预训练如此重要？

##### 如何应用GPT和BERT

使用BERT的模型可以同时考虑一个词的两侧（左侧和右侧）上下文，而不是每次都是只考虑词的单侧（左侧或右侧）上下文。

##### 多语言BERT



## 智能问答

### 智能问答简介

##### 基于知识的问答(Knowledge based)

- 利用知识图谱做问答
- 利用互联网表格做问答

##### 基于文本的问答(Text based)

- 利用互联网文档做问答
- 利用FAQ做问答匹配

### 知识图谱

##### 两类基于知识图谱的问答方法

1. 语义解析方法(Semantic Parsing-Based Approaches)

- 基于语法的语义解析基本框架
- 基于神经网络的语义解析基本框架

2. 答案排行方法(Answer Ranking-based Approaches) 

- 候选答案的选择
- 问题的产生

### 文本推理等技术

- 基于神经网络的匹配
- 预训练技术的应用

### ***总结***：*智能问答正在向常识问答，多轮问答，多语言，少样本场景发展。*



## 机器翻译

### 微软机器翻译的发展历史

- MS Translator Framework
- 微软机器翻译研究的历史
- 在汉英新闻翻译领域达到了专业翻译水平

### 机器翻译的背景

##### 什么是机器翻译

##### 机器翻译发展史上的几个里程碑

1. 1946年，随着计算机诞生，信息论先驱提出了自动翻译的想法
2. 基于规则的机器翻译
3. 基于语料库的机器翻译
   - 基于实例的机器翻译阶段
   - 统计机器翻译(SMT)发展阶段
   - 神经机器翻译(NMT)发展阶段

##### 对机器翻译的评测

BLEU(Bilingual Evaluation Understudy)算法

### 神经机器翻译(NMT)概要

##### 简单神经机器翻译

##### 神经机器翻译的编解码

##### 注意力(Attention)模型的引进

##### 基于RNN的NMT

##### 基于Transformer的NMT

##### NMT的模型训练

- 交叉熵
- 随机梯度下降法

### 机器翻译中的数据增强(Data Augmentation)

NMT的正则化方法

半监督NMT的联合训练(Joint Training)

SMT与NMT的结合

### 在文本语音处理中使用NMT

端到端的自动语音识别(ASR): LAS(Listen, Attend and Spell)

基于Transformer的端到端ASR