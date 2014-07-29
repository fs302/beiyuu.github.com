---
layout: post
title: 龙星计划·机器学习_Day3
category: tech
description: 开课之初余凯就说了，前两天的内容较为基础，后三天的内容偏前沿些。
---
开课之初余凯就说了，前两天的内容较为基础，后三天的内容偏前沿些。

如果说前两天是在细雨绵绵里踮起脚尖过泥潭，从第三天开始，泥水已经漫到腰部了，走一步喘两口气，走得慢的就看不到前面老师拿着的红旗，只能自己摸索了。

lecture 10: [optimization in machine learning (tong)](http://bigeye.au.tsinghua.edu.cn/DragonStar2012/docs/optimization.pdf)

这一节，张潼老师主要讲了机器学习中的寻优问题，也就是——怎样求得最适合的参数w。

主要讲了如下几个方法：



	
  * Gradient Descent（梯度下降）

	
  * Proximal Projection Method（邻近点算法）

	
  * Coordinate Descent（坐标梯度）

	
  * Convex Duality and Dual Coordinate Descent（凸二元和双坐标下降）

	
  * LBFGS


具有凸性的函数（什么是具有凸性呢？）是比较容易求最优解的，因为找到▽f(w)=0，即至少找到一个局部最优解。

梯度下降是求解无约束最优化问题的一种最常用的方法，它是一种迭代寻优的方法，具有简单可靠的优点，迭代过程中η(learning rate)的选择相当重要。张潼老师详细分析了平滑且强凸、平滑但不强凸、不平滑的类型函数η的规模选择。数学太难，我没搞明白……

Proximal Gradient Method（邻近点算法）实际上是将f(w)分成平滑和不平滑的函数，只对平滑函数φ(w)进行梯度下降。

Coordinate Descent （坐标下降法）的idea在于每次只更新一个参数，在logistic问题中，它比梯度下降更加好用。可是，gradient decent不也是每次只更新一个参数吗？哦，这里应该是循环嵌套的不同！CD法是扫描所有数据只为更新一个wi，总共扫描M次；GD法是对于每一个数据，都更新所有wi。这个有点像在线学习(online learning)和批量学习(batch learning)的区别。

lecture 11: [online learning (tong)](http://bigeye.au.tsinghua.edu.cn/DragonStar2012/docs/online.pdf)



	
  * Traditional Online Learning: Perceptron

	
  * Online Convex Optimization

	
  * Stochastic Gradient Descent


最原始的在线学习算法是感知机（Perceptron），即每扫描一个数据，就更新参数值（一般是利用梯度下降法）。

据说在线学习有一个很重要的概念：后悔（regret），它是一个由模型导出的评估函数。我们要做的就是尽量不要后悔。也即make regret as small as possible.

在线学习由于不能回看数据，所以要求一次完成状态的更新。很多批量学习的算法，经过修改，就可以转型online!比如online gradient descent，于是张潼说，有多少批量学习的算法，我们就又能写多少在线学习的paper了，哈哈。

lecture 12: [sparsity models (tong)](http://bigeye.au.tsinghua.edu.cn/DragonStar2012/docs/sparsity.pdf)

我想说的是，这节课我睡着了。。

讲到low-rank（低秩分解）的时候才醒……整节课完全没听懂什么。

lecture 5: [basis expansion and kernel methods (kai)](http://bigeye.au.tsinghua.edu.cn/DragonStar2012/docs/dragonstar_lecture5_nonlinear_svm.pdf)

还是凯哥比较可爱。slides里面有图有真相，给我们这些乡下人下足了料~

kernel是个好东西，它把线性不可分的东西通过加维变换到线性可分了！上一张图：

[![](http://www.wytk2008.net/wordpress/wp-content/uploads/2012/08/kernel.png)](http://www.wytk2008.net/wordpress/wp-content/uploads/2012/08/kernel.png)


上图来自Kai Yu.
