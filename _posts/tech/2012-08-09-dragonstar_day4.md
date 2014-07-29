---
layout: post
title: 龙星计划·机器学习_Day4
category: tech
description: 早上7点起床，从中关村骑车20分钟到清华上课。算是体验了一番北京上班生活的味道~
---
早上7点起床，从中关村骑车20分钟到清华上课。算是体验了一番北京上班生活的味道~

今天余凯老师讲了graphical models，感觉就是讲算法形象化，将变量看作节点（Nodes），将变量间的相互关系看作边。

朱军老师讲了structure learning，但是全堂巨量的英文术语让我完全跟不上，然后就睡下去了。

张潼老师讲Learning on the web，张潼在Yahoo!和IBM的时候，都做过web上的learning，有以下几种类型：



	
  * Classification

	
  * Ranking

	
  * User Behavior Modeling

	
  * Recommendation

	
  * Community Analysis

	
  * Quality Assessment

	
  * Exploration Exploitation

	
  * Scalability

	
  * ...


今天主要讲classification和ranking。明天会cover推荐系统的部分。

Classification有几个要素：

	
  * problem formulation（问题形成）

	
  * feature generation（特征生成）

	
  * information aggregation（信息聚集）

	
  * model adaptation（模型适应）：对于不同的domain作相应的改变适应。


张潼老师说，在做文本分析时，还是用vector space model最好，简单又高效。只需两步：

	
  1. 为文章中出现的每个单词建立字典

	
  2. 用m维向量记录每个单词的frequency


分析：文章中出现次数越少的，可能是越重要的单词。因为出现得多的，像“和、的、有、人、我”，一般都没有太大的意义。我记得TF-IDF法，除了考虑某单词在本篇文章出现的次数，还考虑了在其他文章中出现的频率大小，以评估重要性。

最后一节课，余凯老师讲了最近在语音、视觉处理中大热的Deep learning，它被引入到ML中使ML更接近于其原始的目标：AI。一般的学习算法深度并不大，如SVM的深度是2。深度学习通过增加从输入到输出的层数，来构造更精确的模型。

有兴趣的同学可参考博文：[http://www.cnblogs.com/ysjxw/archive/2011/10/08/2201782.html](http://www.cnblogs.com/ysjxw/archive/2011/10/08/2201782.html)
