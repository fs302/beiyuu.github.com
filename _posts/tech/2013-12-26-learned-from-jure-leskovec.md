---
layout: post
title: Learned from Jure Leskovec
category: tech
description: Jure高中就开始用机器学习研究图像处理并发表文章，本科毕业后到CMU读PhD，博士毕业任Stanford副教授，其大部分论文发表在KDD、TKDE、WWW、ICDM上，目前论文被引已过万。
---
今晚在Videolectures上看了[Jure的博士毕业答辩](http://videolectures.net/sep08_leskovec_tdef/)。Jure高中就开始用机器学习研究图像处理并发表文章，本科毕业后到CMU读PhD，博士毕业任Stanford副教授，其大部分论文发表在KDD、TKDE、WWW、ICDM上，目前论文被引已过万。

去年暑假有幸在北京中科院计算所见过他本人。帅气的外表，风雅的谈吐让人印象深刻。虽然当时基本没听懂他极重口音的演讲，但只看PPT就能感觉到他所做工作的强大。后来一直有关注他的[主页](http://cs.stanford.edu/people/jure/index.html)，非常喜欢他的学术品味。

在跟完他的[CS246:Mining Massive Data Sets](http://www.stanford.edu/class/cs246/)后，现在已经能基本听懂他的演讲了。虽然不是很仔细，但听完他的博士学位答辩，依然收获颇丰。


#### 【科研三范式：观察-建模-应用】


[![Structure](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/Structure.jpg)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/Structure.jpg)完整有意义的科研工作，应该会有以下三个阶段：Observation-Model-Application。

现代意义的观察，不只是列举一系列现象，更深入和有说服力的，应该是实际数据的收集和统计结果的证明——复杂网络和人类动力学方面有很多代表性的工作其实也仅完成了这一步，比如著名的六度分离。

人们能观察到的东西是很有限的，大多数情况下我们所能看到的只是宇宙演化中的一瞬（Snapshot），那么建立模型去尝试解释这个世界变化的原因（比如：哪些人容易成为朋友，盟友间的关系真的稳固吗），对我们看清未来无疑是有极大的好处。于是我们会建立各种演化模型，并且把它和实际数据比较，根据拟合的结果来评判模型的好坏。

当我们的模型已经很接近本质的时候，我们就有能力根据模型去设计算法，通过算法来影响真实的世界，实现对世界的控制（比如对疾病传播的有效预防和切断，查出谣言的来源来打击犯罪，告诉两个互相爱慕很久的人：你们很合适，快在一起吧！）。这是最激动人心的地方，21世纪，人类科学最牛逼的发展就是实现预测。


#### 【透过现象看本质】


[![background](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/background.jpg)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/background.jpg)

网络科学中最重要的图莫过于度分布（degree distribution）了。人们常常讲的幂率分布（长尾分布），就是在度数（x）和其统计值（y）都取对数后，对应关系基本会变成一条直线。深入研究这个现象后，人们提出了优先连接（preferential attachment）的定律，也即富者越富，穷者越穷的网络版解释。如果能从观察中发现特殊，并找出其背后的原因，就是一个很好的研究工作了。


#### 【用数据证明直觉的错误】


[![diameter](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/diameter.jpg)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/diameter.jpg)

直觉上，我们可能会觉得网络的直径（网络中最长的两点距离）应该会随着网络的变大而缓慢增加。而因特网（Internet）和引用网络（Citation）实证结果告诉我们，网络的直径实际上是随着网络规模的增大和逐渐变小的！这也是“小世界”网络的由来，世界越大，我们的隔阂越小——因为我们可以通过更多的渠道进行连接。


#### 【猜想一定要能验证】


[![fit](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/fit-1024x390.jpg)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/fit.jpg)

在研究数据增长机制时，尤其是社会化的数据，常常会出现两个猜想：一是认为数据会衰减式增长，二是觉得数据增长先指数后对数，增长过程中可能会存在引爆点，如果真有这个引爆点，那就有意思了。关于引爆点的研究，跟水结成冰的瞬间是很相似的，物理学家把它叫做渗流（percolation），数学家说是相变（phase transition）。

既然提出了猜想，必定要想办法验证。对于能拿到实际数据的猜想，其最简单的验证方式就是统计，然后作一个带Error Bar的曲线图，实线上的每个点代表x规模所对应的平均值，Error Bar则展示了其误差大小（方差）。这在科研论文中经常见到！


#### 【计算机科学的独特关注点——效率】


[![efficient](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/efficient.jpg)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/12/efficient.jpg)如果说网络和传统的图论没什么两样，网络动力学又和社会、经济学家抢分析的饭碗。那么计算机学派的优势在哪里呢，其中很重要的一点就在于，我们懂得分析效率，这在实际应用中是极其重要的。尤其是很多特性在数据量巨大的条件下才能显现（比如幂率），那么提出最快、最近似的算法，对计算机科学家来说，一定是一块不断追求的香饽饽。
