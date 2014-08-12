---
layout:     post
title:      Science:Clustering by fast search and find of density peak
category: tech 
description: 2014年发表在《科学》杂志上的聚类文章，引发了不少人的关注。一方面是因为这些顶级期刊开始重视数据挖掘领域，另一方面原因是这个聚类算法的简洁高效。
published: false
---

![Aggregation-Cut off Kernel][1]

DATA:http://cs.joensuu.fi/sipu/datasets/

CODE:
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
