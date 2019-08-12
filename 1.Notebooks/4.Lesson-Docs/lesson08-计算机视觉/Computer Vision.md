# 计算机视觉(Computer Vision)

> 每一幅图像都在讲述一个故事。

## 计算机视觉简介

计算机视觉的目标

- 图上有什么实体？
- 他们在哪里？
- 他们在干什么？
- 他们在图上的位置？

计算机视觉处理的常见困难

- 同类物体不同大小范围
- 同一个物体的不同视角
- 遮挡
- 光线造成的阴影

### 数字图像的表达

1. 图像的维度
2. 图像的清晰度
3. 图像的格式
4. 彩色图像的表达

### 人类眼睛的视觉处理系统

眼睛产生的错觉

### 基本图像处理和构建

信号传输，过滤器，图像修复，图像锐化，边缘检测等

### 三维重建

双视角重建，多视角重建，运动重建，极线的概念

<img src ="..\Images\epipolar geometry.JPG">

### 图像识别

图像分类，目标定位，目标识别，图像分割

<img src ="..\Images\image recognition.png">



### 图像处理的应用与挑战



## 图像识别

### 背景知识

- 空间变换 - 计算机视觉一个长期存在的问题
- 传统的思考方法
- CNN（卷积神经网络）的应用

### 空间变换网络（Spatial Transformer Network ）

在STN学习和训练的过程中，学习到的空间变换网络会自动提取局部数据特征，并对提取到的数据进行空间变换，从而得到输出。

<img src ="..\Images\spatial transformer.png">

### 可变形卷积网络（DCN）

传统卷积窗口只要训练每个卷积窗口的像素权重参数即可。而可变形卷积网络对同样的窗口必须外加一些参数，用来训练卷积窗口的形状，大大增强了CNN的几何变换建模能力。

##### 一，可变形卷积

在标准卷积中向常规采样网格添加了2D偏移。

<img src ="..\Images\deformable-convolution.png">

##### 二，可变形RoI Pooling

在标准RoI pooling的常规bin partition中为每个bin添加了一个offset。

<img src ="..\Images\RoI Pooling.png">

### 可变形卷积网络V2

1. DCNV2用了更好的visualization来更深入的理解DCNV1为什么work
2. 增加更多的Deformable Convolution
3. 让Deformable Convolutor不仅学习offset，还能学习每个采样点的权重（modulation）
4. 模拟R-CNN的feature（knowledge distillation）

<img src ="..\Images\R-CNN feature.jpg">

### 视频识别

视频识别的困难：计算量太大造成逐帧识别难以实现，模糊/遮挡/少见姿势等难以识别。

##### 稀疏特征传播 (Sparse Feature Propagation)

由于视频中相邻帧之间包含冗余信息，因此不需要在每一帧中提取feature map。微软提出了**DFF**(Deep Feature Flow)， 主要在少数关键帧中提取feature map，然后将这些feature map传播到其他非关键帧上。在牺牲少量精度的情况下，对检测器进行加速。

<img src ="..\Images\sparse feature propagation.jpg">

##### 多帧特征聚合 (Multi-frame Feature Aggregation)

第一次在视频目标检测领域引入时序特征聚集 FGFA(Flow-Guided Feature Aggregation)的概念，某些帧的深度特征会受到外观衰退的影响(如运动模糊，遮挡等)，但是可能通过将多个邻近帧进行聚合，提高特征的质量和检测准确率。

<img src ="..\Images\FGFA.jpg">

##### Towards High Performance Video Object Detection (for Mobiles)

在遵循上述两个原则(1, Sparse Feature Propagation; 2, Multi-frame Feature Aggregation) 的前提下，适用于手机端视频目标检测的轻量级神经网络。



## 人物与物体跟踪

视觉追踪是人类最基本的视觉能力之一。对计算机视觉而言，视觉追踪是视频分析的基础模块，也是人机交互、智能监控等应用的技术基石。

##### 应用场景分类

- 单摄像头 - 各种视频处理
- 双摄像头 - 机器人
- 多摄像头 - 环境监控

##### 技术分类

- 单目标 - 任意物体或物体类别
- 多目标 - 通常是固定某一类别，其中人与车是最受关注的

### 单目标跟踪

定义：在一段视频的第一帧给出目标物体的边框，要求在接下来的每一帧给出目标的边框。

通常所说的Visual Object Tracking (VOT)，均指单目标跟踪。

##### VOT的演进

##### VOT的技术路标

1，The KLT (Kanada-Lucas-Tomasi) Tracker

2，相关滤波算法 (Correlation Filter)

- Correlation Filter 最早应用于信号处理，用来描述两个信号之间的相关性，其运行机制主要是追踪 + 滤波器训练。

3，SiamFC

全卷积Siamese (Full Convolutional Siamese) 

- CNN 用于特征提取
- 当前帧搜索做相似度匹配

<img src ="..\Images\SiamFC.png">

- 最后得到一个响应图，图上最大值对应的点就是算法认为的目标中心所在位置。

4，SA-Siam

使用双重孪生网络进行视觉物体跟踪，融合语义和外观信息的SA(Semantic Appearance)-Siam框架。

<img src ="..\Images\sa-siam.png">

5，Siam-BM(Better Match)

- 在SASiam上做了改进，添加了旋转角度模块来预测目标的旋转

6，最新进展

- SPM-Tracker
- SiamRPN和SiamRPN++
- ATOM

### 人物跟踪

##### 多目标跟踪

匈牙利算法

马尔可夫决策流程

##### Binocular people following

基于RGBSD数据的跟踪

##### Pose estimation and tracking

多人姿态推测

- 自上而下的方法：先检测人，再检测人的姿态
- 自下而上的方法：先扫描全图，再检测哪部分属于目标群体

##### Person Re-identification(Re-ID)

多视频多角度的人物跟踪，端到端的个人定位系统



## 生成对抗网络及其应用

识别模式和生成模式

<img src ="..\Images\GAN.png">

### GAN是如何工作的

1. 密度估算
2. 样本生成

### GAN的应用

1. 人脸生成
2. 根据遮挡部分生成图像
3. 由素描生成彩色图像
4. 生成高解析图像
5. 跨类别的图像生成，例如由照片生成油画，同一地点的四季交替等等
6. 生成多种属性图像

<img src ="..\Images\GAN face.jpg">

### 保留身份信息的人脸合成技术

希望解决开放数据集中人脸生成的问题，合成保留图中人脸原始特点的图像。针对输入的人脸图片，我们希望从中得到身份(Identity)和属性(Attributes)信息。基于GAN的框架实现人脸生成。

<img src ="..\Images\GAN-attributes.png">

### GAN新应用的探索

黑白视频上色

图像属性的改变

目前尚待突破的问题等