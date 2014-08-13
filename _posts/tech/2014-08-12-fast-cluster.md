---
layout:     post
title:      Science上发表的简单快速的聚类方法
category: tech 
description: 2014年发表在《科学》杂志上的聚类文章，引发了不少人的关注。一方面是因为这些顶级期刊开始重视数据挖掘领域，另一方面原因是这个聚类算法的简洁高效。于是在工作之余，对这个算法进行了理解和实现。并不感觉它有多高大上，但是在两个关键问题上，还是有所得。
published: true
---
工作以后发现自己学习和研究的时间变得少得可怜。

前两周因为一个同事的交流，关注了一下[canopy](http://en.wikipedia.org/wiki/Canopy_clustering_algorithm)辅助Kmeans聚类确定簇数目。然后想起最近很火的一篇Science文章：[Clustering by fast search and find of density peak](http://wenku.baidu.com/link?url=5Wvn42-wj0z3UzyXeZNdLc3OpFnmGE7LsNU2Z3I1GWN3vSg8oy1Ub_QGSJISt7rVMlOSYgeodXQrU7ukUJGGgKm3yFzIgVYc8YupLdtlPh7),据说非常简单而优美。然后上网上搜了一下，评论的文章也就那样转来转去，其实就是把人家论文拿来翻译一下，有些关键点根本没讲清楚，真不知道翻译者是不是自己实现过那个算法。

我之所以对这个算法感兴趣，主要是因为看到论文中可以识别那么奇形怪状的点簇，然后又只有两个指标，好像很有道理又很好算的样子。没想到被坑惨了，我用了差不多两个星期，偶尔下班后有时间看论文、写代码，才把这个简单的算法实现下来。其中依然还有一个参数需要手工调整，就是delta的阈值（下面有讲）。

**<center>图1 聚合点簇Aggregation（共7个聚类，采用Cut off kernel）</center>**
![Aggregation-Cut off Kernel][1]

这个算法，其实是对所有坐标点，基于相互距离，提出了两个新的属性，一是局部密度rho，即与该点距离在一定范围内的点的总数，二是到更高密度点的最短距离delta。作者提出，类簇的中心是这样的一类点：它们被很多点围绕（导致局部密度大），且与局部密度比自己大的点之间的距离也很远。

**<center>图2 A sets（20个聚类，算法分出19个，采用Cut off kernel）</center>**
![A sets-Cut off Kernel][2]

聚类过程，[这篇文章](http://www.52ml.net/16296.html)讲的还不错，首先我们要把所有点的两个属性算出来，可以画出一个平面决策图；根据决策图，选出rho和delta都大的点作为聚类中心；选定中心后，让周围的点采取“跟随”策略，归类到密度比自己大的最近邻居所在的簇。
我更想说的是两点，第一点关于聚类中心的选择，我翻看了几篇文章（包括作者原文），都没有明确指出一个很好地自动确定中心数目的方法，较多的做法是画出决策图后进行人工选定范围。我的做法是，按rho排序，然后根据决策图人工对delta取一个合适的值，大于这个delta值的，才能被选为中心。这种做法很人肉，让我一直有没完结的感觉，继续探索一下有没有自动确定这个阈值的方法。

**<center>图3 火焰形状（2个聚类，采用Cut off kernel）</center>**
![Flame-Cut off Kernel][3]

第二点是关于rho的计算，其实论文中只提到一个计算公式，是通过截断距离做线性判断，即rho=sigma(sign(dij-dc)),这个计算方法对一般的球状簇，如图[1]，图[2]，有不错的效果，而且计算快速，但是对图[3]的异形图（类簇形状并不呈球状分布），效果就不好了。这时候翻看作者给出的matlab源码，了解到还可以使用高斯核(Gaussian Kernel)函数来定义局部密度，引入以后就完全handle了异形的问题，见图[4]，我觉得高斯核函数确实是强大（但是为什么呢？）。具体实现可以参看python代码（两种rho的计算都有）。

**<center>图4 火焰形状（2个聚类，采用Gaussian kernel）</center>**
![Flame-Gaussian kernel][4]

DATA来源:[http://cs.joensuu.fi/sipu/datasets/](http://cs.joensuu.fi/sipu/datasets/)

[CODE:flame_GaussianKernel.py]

    import math

    def getDistance(pt1, pt2):
        tmp = pow(pt1[0]-pt2[0],2) + pow(pt1[1]-pt2[1],2)
        return pow(tmp,0.5)

    def ChooseDc(dc_percent,points,dis,distance):
        avgNeighbourNum = dc_percent*len(points)
        
        maxd = 0
        for i in range(0,len(points)):
            for j in range(i+1,len(points)):
                pt1 = points[i]
                pt2 = points[j]
                d = getDistance(pt1,pt2)
                dis.append(d)
                distance[i,j] = d
                dis.append(d)
                distance[j,i] = d
                if d>maxd:
                    maxd = d
        dis.sort()
        return dis[int(avgNeighbourNum*len(points)*2)]

    def drawOriginGraph(pl,points,cl,colorNum):
        x = [xx for (xx,yy) in points]
        y = [yy for (xx,yy) in points]
        cm = pl.get_cmap("RdYlGn")
        for i in range(len(points)):
            pl.plot(x[i],y[i],'o',color=cm(cl[i]*1.0/colorNum))

    def drawDecisionGraph(pl,rho, delta,cl,colorNum):
        cm = pl.get_cmap("RdYlGn")
        for i in range(len(rho)):
            pl.plot(rho[i], delta[i],'o',color=cm(cl[i]*1.0/colorNum))
        pl.xlabel(r'$\rho$')
        pl.ylabel(r'$\delta$')
        
    def Cluster():
        #=========Load Data=========
        InputFileName = "flame"
        OutputFileName = InputFileName + "_out"
        suffix = ".txt"

        Fin = open(InputFileName+suffix,"r")
        Fout = open(OutputFileName+suffix,"w")

        points = []
        for line in Fin.readlines():
            data = line.split()
            if len(data)==3:
                a = float(data[0])
                b = float(data[1])
                points.append((a,b))

        #=========Calculating=========
            #-----choose dc-----
        dc_percent = 0.015
        dis = []
        distance = {}
        dc = ChooseDc(dc_percent,points,dis,distance)
        print("dc:"+str(dc))

            #-----cal rho:"Cut off" kernel
        '''
        rho = [0 for i in range(len(points))]
        for i in range(0,len(points)):
            for j in range(i+1,len(points)):
                dij = getDistance(points[i],points[j])
                if dij<dc:
                    rho[i] += 1
                    rho[j] += 1
        '''
            #-----cal rho:"Gaussian Kernel"
        rho = [0 for i in range(len(points))]
        for i in range(0,len(points)):
            for j in range(i+1,len(points)):
                dij = getDistance(points[i],points[j])
                rho[i] += math.exp(-(dij/dc)*(dij/dc))
                rho[j] += math.exp(-(dij/dc)*(dij/dc))
               

        rho_list =[(rho[i],i) for i in range(len(rho))]
        rho_sorted = sorted(rho_list, reverse=1)
        print("Highest rho:",rho_sorted[0])

        maxd = dis[-1]
        delta = [maxd for i in range(len(points))]
        nneigh = [-1 for i in range(len(points))]
        for ii in range(1,len(rho_sorted)):
            for jj in range(0,ii):
                id_p1 = rho_sorted[ii][1] #get point1's id
                id_p2 = rho_sorted[jj][1] #get point2's id
                if (distance[id_p1,id_p2]<delta[id_p1]):
                    delta[id_p1] = distance[id_p1,id_p2]
                    nneigh[id_p1] = id_p2

        #assignment
        cl = [-1 for i in range(len(points))]
        colorNum = 0
        for ii in range(len(rho_sorted)):
            id_p = rho_sorted[ii][1]
            if (cl[id_p] == -1 and delta[id_p]>7.0):
                cl[id_p] = colorNum
                colorNum += 1
            else:
                if (cl[id_p] == -1 and cl[nneigh[id_p]!=-1]):
                    cl[id_p] = cl[nneigh[id_p]]
        print(colorNum)

        import pylab as pl
        fig1 = pl.figure(1)
        pl.subplot(121)
        drawOriginGraph(pl,points,cl,colorNum)
        pl.subplot(122)
        drawDecisionGraph(pl,rho,delta,cl,colorNum)
        pl.show()

        for i in range(len(points)):
            Fout.write(str(i)+","+str(rho[i])+","+str(delta[i])+"\n")

        #Assign Cluster

        Fin.close()
        Fout.close()

    if __name__=="__main__":
        Cluster()


[1]:http://findshine.qiniudn.com/figure_Aggr_cutoff.png "Aggregation-Cut off Kernel"
[2]:http://findshine.qiniudn.com/figure_a1_cutoff.png "A sets-Cut off Kernel"
[3]:http://findshine.qiniudn.com/figure_flame.png "Flame-Cut off Kernel"
[4]:http://findshine.qiniudn.com/figure_flame_Gaussian.png "Flame-Gaussian kernel"
