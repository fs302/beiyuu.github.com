---
layout: post
title: 用Twitter挖掘“阿拉伯之春”之革命因果
category: project
description: 【阿拉伯之春】
---
【阿拉伯之春】

**阿拉伯之春**（[阿拉伯语](http://zh.wikipedia.org/wiki/%E9%98%BF%E6%8B%89%E4%BC%AF%E8%AF%AD)：الثورات العربية‎）是西方媒体所称的[阿拉伯世界](http://zh.wikipedia.org/wiki/%E9%98%BF%E6%8B%89%E4%BC%AF%E4%B8%96%E7%95%8C)的一次[革命](http://zh.wikipedia.org/wiki/%E9%9D%A9%E5%91%BD)浪潮。自2010年12月份[突尼斯](http://zh.wikipedia.org/wiki/%E7%AA%81%E5%B0%BC%E6%96%AF)一些城镇爆发动乱以来，[阿拉伯世界](http://zh.wikipedia.org/wiki/%E9%98%BF%E6%8B%89%E4%BC%AF%E4%B8%96%E7%95%8C)一些国家民众纷纷走上街头，要求推翻本国的专制政体的行动，并乐观地把“一个新[中东](http://zh.wikipedia.org/wiki/%E4%B8%AD%E4%B8%9C)即将诞生”预见为这个运动的前景，认为这个“阿拉伯之春”属于“谙熟互联网、要求和世界其它大部分地区一样享有基本民主权利的年轻一代”。（[维基百科](http://zh.wikipedia.org/zh/%E9%98%BF%E6%8B%89%E4%BC%AF%E4%B9%8B%E6%98%A5)）

[![Arabic_protests](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/03/549px-Info_box_collage_for_mena_Arabic_protests.png)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/03/549px-Info_box_collage_for_mena_Arabic_protests.png)

【数据说明】

利用Twitter提供的[Search API](https://dev.twitter.com/docs)，首先查询“Arab Spring”得到1000个近期tweets，提取所有单词并按出现频率排序得到Top100的单词表。手工筛选出30个有意义的单词（除去in/on等介词，you/I等代词）重新进行一批爬取，对于每个单词作一次查询，同样得到一个频繁单词集合（前1000）。

【数据可视化】

建立单词的**引用网络**，即：节点表示一个单词组，有向边表示引用情况，比如查询CNS时，News在相关tweets中出现了381次，则有CNS指向News权重为381的边。

[![ArabSpring](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/03/ArabSpring-1024x1024.png)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/03/ArabSpring.png)称上面的网络为Tags-network，该网络中有30个节点，204条有向边。

【网络分析】

从关键词网络中，Arab Spring和News是两个度最大的节点。几个比较明显的路径有：

(1)Arab Spring->self-immolation->Tunisian->protest

(2)CNS->News

(2)Arab Spring->Intelligence(情报)

<!-- more -->

第1条路径指向了整个“阿拉伯之春”的导火索——突尼斯(Tunisian) 26岁年轻人穆罕默德·布瓦吉吉因失业而抗议自焚（self-immolation），引起了突尼斯全国范围的骚乱（protest）。

第2条中的CNS作为一个提供给新闻工作者和广播团体发布新闻的网站，在其twitter或者站点发布了大量与阿拉伯之春有关的报道与分析。

同样上榜的还有@BBC_WHYS（http://www.bbc.co.uk/blogs/worldhaveyoursay/）和Broadcasting World（http://www.broadcastingworld.net/）

第3条由阿拉伯之春指向“情报”（Intelligence）的边说明了在此事件进行和后续分析中，情报信息的重要性，也为我们进一步分析革命的爆发原因提供了指向。

初步网络结构分析后，我们再从tag的属性入手。

网络中提到两个国家：突尼斯(Tunisia)和埃及(Egypt)，它们是“阿拉伯之春”事件的先驱并且有很多共性（如领导人长期在位），此两国应当成为分析的重点。

穆斯林（Islamists）也是图中显现出来的一个重要关键词，伊斯兰教的因素，在此次风暴中，扮演了什么样的角色？阿拉伯之春对穆斯林是有益（Benefited）还是有害？为何twitter中有这么大的比重提到Benefited这个词？

经济（Economics）、自由（freedom）和改革（change）在图中出现，表示了这三个因素在此事件中的极大影响，简单来说，就是国民经济衰退，人权的侵犯、国家领导人不思改革等综合因素引发的大规模革命。

还有几个很奇怪的词，就需要我们借用搜索引擎来进行辅助分析了。

(1)Cleveland/ Social/Host/Networking

(2)Jeremy/Scott

(3)#tcot/opening/Collection

从Google的结果来看：

(1) **Cleveland State Community College** will be hosting The Arab Spring: Social Movements and Social Networking lecture on Monday, March 25 at 2 p.m. in the Library, Room 118. 原来是Cleveland的这所大学即将举办一个专题演讲，近期在twitter上做了宣传。

(2)Jeremy Scott是一款时装品牌。他们最近推出了一个系列叫Arab Spring！这个就跟我们研究的问题没有直接的关系了。（[http://www.jeremyscott.com/](http://www.jeremyscott.com/)）

(3) **#tcot **- Top Conservatives On Twitter：A collection of conservative thoughts and people on twitter.

结合我们爬取到的数据，尝试追踪和回答下面几个问题。

1.为何革命爆发如此之突然，如此之迅速？

我认为有两个影响因素，一是社会网络媒体的参与，二是穆斯林共享的宗教信仰条件。

“阿拉伯之春”亦被冠名推特革命（Twitter revolution），Facebook/twitter在信息发布、集结、响应上发挥了巨大的作用，“六度分割”也告诉我们，信息在小世界网络中传递的速度是惊人的！之所以官方并未察觉以及采取有效的压制措施，可能是其信息部门的失责，若是换成美国，此类事情恐怕很快就会被block！

而穆斯林共同的宗教信仰，也在一定程度上助力了躁动的集结速度。

2.为何君主制阿拉伯国家免于风暴，独善其身？

我想这一点也与穆斯林国家的特质有关。君主制国家被授予了神的权力，在宗教信仰的限制下，并不容易爆发自下而上的革命，从而在此风暴中独善其身。
