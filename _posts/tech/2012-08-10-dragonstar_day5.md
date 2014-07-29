---
layout: post
title: 龙星计划·机器学习_Day5
category: tech
description: 今天的课程内容全部由余凯老师讲授。主要讲了迁移学习（Transfer learning ），半监督学习（Semi-Supervised learning），推荐系统（Recommendation System）和计算机视觉（Computer Vision）。
---
今天的课程内容全部由余凯老师讲授。

主要讲了迁移学习（Transfer learning ），半监督学习（Semi-Supervised learning），推荐系统（Recommendation System）和计算机视觉（Computer Vision）。

【迁移学习】

迁移学习我以前接触过，学做交叉推荐时看过几篇利用矩阵分解做迁移学习的内容，知道迁移学习很好的利用了不同系统（Domain）间的相似性，可以相当神奇的填充矩阵的缺失值。以前是由交叉推荐才接触到迁移学习，现在听过课，感觉视野就变大了，知道迁移学习可以做的东西还有很多。

余凯老师用三句话说明为什么会有迁移学习这东西？（Why transfer learning?）



	
  * In some tasks training data are in short supply.

	
  * In some domains, the calibration effort is very expensive.

	
  * In some domains, the learning process is time consuming.


更广泛地说，比如在日耳曼语系中，德语就可以迁移到荷兰语，因为他们有相同的起源，就很有可能有相似的语言习惯。又比如，在computer vision中，一个对汽车识别很好的模型，也可以被迁移到识别自行车的模型中（因为它们都有轮子），以辅助加快识别效果。

迁移学习还有其他名字，比如Multi-task learning以及learning to learning。其实我们在做regression的时候，就有用到集体智慧，共同学习的思想，只不过迁移学习将这个思想应用到不同的系统中，产生了巨大的效应！

迁移学习有一篇综述A survey on transfer learning是杨强等人完成的，有机会可以去看下。

【半监督学习】

Semi-Supervised Learning是一个很有趣的approach，它本着充分利用数据的原则，把驴子当马也派上了用场。由于labeled数据在大系统中占的比例相当小，而unlabeled数据也许有很多。

[![Results with and without unlabeled data](http://www.wytk2008.net/wordpress/wp-content/uploads/2012/08/semi.png)](http://www.wytk2008.net/wordpress/wp-content/uploads/2012/08/semi.png)

如上图所示，如果只利用labeled数据做分类，那么得到的分类线是一条不那么漂亮的弧线，而右边加入unlabeled数据后，我们发现它们的分布是呈左右对称的，于是可以画出一条中间横切的分割线，那样得到的效果就好多了。

【推荐系统】

推荐系统我们讲过很多，推荐的关键实际上就是如何最好地填充用户和项目的“评分矩阵”缺失值。以后有机会，我准备专门写一篇文章谈谈我的理解。

最后一节课是一个forum，几个做ML比较好的学者被邀请到上面回答学生的问题。最后他们给机器学习的研究者推荐了几本经典书籍，其中常老师和张长水老师推荐的书记较适合入门，其他书籍相对难搞。

附上书名：

常虹：pattern recognition and machine learning
张长水：pattern classification
朱军：graphical model
张潼：<del>elements of stochatical learning</del> the elements of statistical learning(ESL)
王利伟： the nature of statistical learning theory
