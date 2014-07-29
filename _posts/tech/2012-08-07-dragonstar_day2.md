---
layout: post
title: 龙星计划·机器学习_Day2
category: tech
description: 今天的课程主要围绕model一词进行串讲。
---
今天的课程主要围绕model一词进行串讲。

什么是model？model可以理解成算法，只是 model比算法的概念小一些。采用相同的算法思路，但使用不同的参数，就可以构造出不同的model。而model的合理应用（select or combine），是机器学习的关键。

余凯老师讲了Model Selection（模型选择），张潼老师讲Model Combine（集群学习），并将selection和combination作了一系列详细的分析。最后，张潼老师做了一个关于Learning Theory的Overview。

**lecture 6: model selection and evaluation (kai)**

余凯从多项式拟合的例子出发，用一系列拟合图（0st-3st-9st，_st表示变量最高次数_）分别表示出（underfit-just right-overfit）的情况。用期中考试的例子，指明过拟合并非好事：就像老师出期中考题，是否会拿家庭作业中的原题作为考题重现呢？这样能准确测试出学生的学习效果吗？聪明的老师，如果他有100题的题库，那么他顶多拿80题给学生做练习，另外20题得留来考试用。这就是我们在做机器学习时常用的方法，引入Validation Set：将训练数据划分成3类：[training set/validation set/test set]，在learning过程中，仅使用validation set作为“测试集”，而先忽略test set。

由于这样一个数据划分，使得真正用于训练的数据变少，尤其在数据量不够的情况下，这是一项巨大的损失（You waste some portion of your data），怎么办？Recycle Data！

于是我们引入了Cross Validation！通俗地讲，如果把每个数据比作一个员工。那哪个员工都应该有义务值班！这样好了，我们轮班嘛。这样每个人都上前线，每bit数据都派上用场！Cross Validation主要讲了两类：



	
  * LOOCV(leave-One-Out Cross Validation)




所谓“留一法”，就是每次拿一个数据作为测试数据，其余数据都用作训练。对于每个model作效果评估，如此遍历下来求个平均即可得到此model的终极水平！但是你应该也发现了，这样**计算量大，特别耗时**！！






	
  * K-fold Cross Validation




K-fold呢，发现了LOOCV的弱点，于是尝试弥补之。我们只需要将数据分成K块（K-fold），每一块分别用作Validation。这样呢，evaluation的重复次数从M（数据点数）下降到K，应该是一种不错的改进了。


附上model selection的分类：



	
  *  In-sample error estimates:




– Akaike Information Criterion (AIC)
– Bayesian Information Criterion (BIC)
– Minimum Description Length Principle (MDL)
– Structural Risk Minimization (SRM)






	
  *  Extra-sample error estimates:




– Cross-Validation
– Bootstrap


**lecture 7: model combination (tong)**

combine,Also called ensemble or blending or averaging.



	
  * Equally weighted averaging (voting).

	
  * Exponentially weighted model averaging.

	
  * Weight optimization using stacking.

	
  * Bagging.

	
  * Additive model and boosting


Model Selection versus Averaging（选择single model还是combine model？）

	
  * Model selection: works better if one model is significantly more accurate than other models




当其中一个model远胜于其他model，那就用single model吧！而且这样做出来的结果具有较好的解释性（interpretable），但不如集群model那么stable。






	
  * Equally weighted averaging: works better if all models have similar prediction accuracy, but are different




如果所有model表现相似（预测效果差距不大），但是偏好不同，合并使用后会有明显的提高，那么则使用集群学习。


**lecture 8: boosting and bagging (tong)**

stacking/bagging/boosting都是集群学习的方法。需要更多的学习，此处暂且不叙。

Adaboost（adaptive boosting）是一个极其经典的方法，它的提出使得学术界开始重视boosting，并且引发了大量研究。

我今天才察觉bootstrap就是周涛说的自助统计，为何称其为“自助”，还不太理解，待看。

决策树和关联规则，也是机器学习的基础且重要的部分，以前重视不够，听过老师讲后发现可以研究和应用的地方还挺多的！

**lecture 9: overview of learning theory (tong)**

评判一个学习算法的好坏，不能完全指望它在训练集上的表现。我们已经可以很明确的知道，即便我在训练集上写出一个precision百分百的函数，也不敢保证它在将来新数据的表现中能有多好的表现。如何利用现有的数据，尽量期望做出一个“未来期望”最好的模型，是我们的终极任务！

数据集的分类（training set/test set）就是一种自我评估的先驱和基础。因而对Training ERROR和Test ERROR的利用和评估，是提高算法的关键途径。只有在TEST集的ERROR控制好，才能有好的“未来表现”。这两个ERROR，会有一个Curve，值得我们仔细研究！

uniform convergence指出了How Close is Test Error to Training Error，从概率的角度提出了两个ERROR的可比性。

Some Common Techniques for Uniform Convergence：



	
  * Exponential Probability Inequality (Chernoff Bound)

	
  * VC dimension

	
  * Covering numbers

	
  * Rademacher Complexity


