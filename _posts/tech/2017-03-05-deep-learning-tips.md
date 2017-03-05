---
layout: post
title: 深度强化学习系列笔记：深度神经网络
category: tech 
description: 本文介绍了深度学习的基本流程，优化方法，以及两个极具代表性的深度神经网络CNN和RNN

---

2016年以来，随着AlphaGo的声名鹊起。深度学习和强化学习在学术界和工业界掀起了波澜大风，在人们探索通用人工智能的路上，他们就像黑白双煞，一个负责特征理解，一个负责找训练粮食。从去年12月起，我们团队也投入大量精力研究和学习DRL领域的技术和算法，并尝试用在电商推荐和智能投放领域。经过3个月的学习，基本建立起深度学习和强化学习的框架，我计划用2篇文章来梳理一下学习到的内容，并尝试做一些扩展。

这是第一篇文章，主要梳理深度学习的算法理论框架。

以下是根据台湾大学李宏毅教授的讲稿《Deep Learning Tutorial》整理的提纲：

### Introduction

* Steps for Deep Learning
    1. define a set of function [Neural Network]
    2. goodness of function [Loss]
    3. pick the best function [Back propagation]

### Training DNN

* Good on Training Data
    - Choosing proper loss
        + Square Error
        + Cross Entropy
    - Mini-batch
        + Faster
        + Avoid Overfitting
    - New activation function
        + Vanishing Gradient Problem
        + Rectified Linear Unit(ReLU)
        + Maxout[Ian J. Goodfellow, ICML'13]
    - Adaptive Learning Rate 
    - Momentum[Adam]
* Good on Test Data
    - Early Stopping
    - Regularization
    - Dropout(is a kind of ensenble)[Ian J. Goodfellow, ICML'13]
    - Network Structure

### Variants of Neural Networks

* Convolutional Neural Network(CNN)
    - Why CNN for image
        + patterns are much smaller than the whole image [Convolution]
        + same patterns appear in different regions [Convolution]
        + subsampling the pixels will not change the object [Max Pooling]
    - Process: Convolution & Max Pooling & Fully Connect
* Recurrent Neural Network(RNN)
    - The output of hidden layer are stored in the memory
    - Long Short-Term Memory(LSTM) [4 inputs,1 outputs]
        + Input Gate 
        + Signal control the input gate
        + Forget Gate
        + Signal control the output gate
