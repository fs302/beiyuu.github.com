---
layout: post
title: 龙星计划·机器学习_Day1
category: tech
description: 《机器学习》课程开课第一天。很顺利地占到了座位，并且完成了第一天的学习。课程容量相当大，从上午9点到下午2点持续上课，虽然累，但是很有收获！
---
《机器学习》课程开课第一天。很顺利地占到了座位，并且完成了第一天的学习。课程容量相当大，从上午9点到下午2点持续上课，虽然累，但是很有收获！




**lecture 1: Introduction to ML and review of linear algebra, probability, statistics (kai)**




余凯博士介绍了本次课程的目标：Excited-Overview-Distinguish-Design.首先**激发**大家对机器学习的热情，然后从**整体的、系统的**角度介绍机器学习领域的各项技术，接着让大家学会**区分**使用不同的方法，最后懂得**设计**合适的机器学习系统。




在第一讲中，他提到了语音识别技术在学术界和工业界的起伏历程。80年代的提出并受到关注，90年代的没落（由于工业界不够重视）再到如今随着智能手机、语音输入法的发展的炙手可热，语音识别是机器学习的一个重要分支。在学术界，也许将准确率提升一点就可以发论文，并不在意它的鲁棒性，而业界发展产品时，却极致要求完满。所以，从科技发展的角度来看，学术上的创新+工业中的严格要求才能造就非凡的产品。




余凯提出了机器学习的三大要素：数据、模型、算法。但他也指出，数据的预处理效果往往决定了算法表现的Upper bound，这有时候倒是我们不太注意的地方。




**lecture 2: linear model (tong)**




张潼是做统计出生的，讲的东西比较偏数学。




两位专家都强调了线性模型（Linear model）的重要性，因为很多非线性的结构可以用线性模型的方法（Method）来处理，也就是说，只需要对参数β是线性的，其变量可以是非线性的。




**lecture 3: overfitting and regularization (tong)**




本节重点讲了如何寻找最重要的特征（variable），一是人工分析，二是动态调配：通过增加或减少variable，观察预测效果的变化来判别其重要性。




在处理overfitting问题上，主要也是两种方法：一是选择重要的features（人工选择或者用model selection algorithm），二是regularization，通过设置λ（regularization parameter）来削弱参数的影响力。




他重点分析了Lp Regularization和Ridge Regression。我都没听懂……




**lecture 4: linear classification (kai)**




午饭回来，余凯博士讲了线性分类器。主要说了感知机(Perceptron Algorithm)和支持向量机(Support Vector Machine)。课上他提到的Deep learning是一个新概念，我只听说是神经网络的一个发展，有空去具体了解一下。余凯博士在Summary的时候还提出，Large Scale Linear Classification最近又成为一个热门的研究课题，目前还没有很好的解决，也许是一项值得挖掘的领域。




课程刚刚开始，全场400+人，本科生不到10个，略感压力大。不过认识了一些博士生（清华、中科院自动化所等），有很大的交流空间和共同兴趣，相信接下来会有更大的收获。
