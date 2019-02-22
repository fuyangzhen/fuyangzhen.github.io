---
layout: post
title: "Gradient descent & logistic regression"
tags: [ml, math]
comments: true
date: 2016-10-13 00:00:00
---

## 1 简介
这一章的 [机器学习实战(第5章)](http://www.ituring.com.cn/book/1021) ，唯一想说的就是里面关于梯度上升算法的细节问题。  

<!--more-->

以下程序需要注意`python`的`list`格式和`numpy`的`array`及`matrix`(矩阵计算时需要使用该格式) 格式的互相转化问题。  

数据加载处理函数：

```py
def loadDataSet():
    dataMat = []; labelMat = []
    fr = open('/Users/fuyangzhen/Documents/machinelearninginaction/Ch05/testSet.txt')
    for line in fr.readlines():
        lineArr = line.strip().split()
        try:
            dataMat.append([1,float(lineArr[0]), float(lineArr[1])])
            labelMat.append(int(lineArr[2]))
        except ValueError as e:
            print("error:", e, "on line", line)
    return dataMat, labelMat
```  
sigmoid函数：

```py
def sigmoid(inX):
    return 1/(1+np.exp(-inX))
```  
逻辑回归梯度上升优化算法：

```py
def gradAscent(dataMatIn, classLabels, Cycles = 1000):
    dataMatrix = np.mat(dataMatIn)
    labelMat = np.mat(classLabels).T
    m,n = np.shape(dataMatrix)# (100, 3)
    alpha = 0.001
    maxCycles = Cycles
    weights = np.ones((n,1))
    for k in range(maxCycles):
        h = sigmoid(dataMatrix * weights)
        error = (labelMat - h)
        weights = weights + alpha * dataMatrix.T * error # 此处的梯度上升算法的数学细节被作者完全省略
    return weights
```  
散点图及决策边界绘图函数：

```py
def plotBestFit(weights):
    weights = array(weights)
    dataMat, labelMat = loadDataSet()
    dataArr = array(dataMat)
    n = np.shape(dataArr)[0]
    xcord1 = []; ycord1 = []
    xcord2 = []; ycord2 = []
    for i in range(n):
        if int(labelMat[i]) == 1:
            xcord1.append(dataArr[i,1]); ycord1.append(dataArr[i,2])
        else:
            xcord2.append(dataArr[i,1]); ycord2.append(dataArr[i,2])
    fig = plt.figure()
    ax = fig.add_subplot(111)
    ax.scatter(xcord1, ycord1, s=30, c='red', marker='s')
    ax.scatter(xcord2, ycord2, s=30, c='blue')
    x = np.arange(-3, 3, 0.1)
    y = (-weights[0]-weights[1]*x)/weights[2]
    ax.plot(x, y, 'g')
    plt.xlabel('X1'); plt.ylabel('X2')
    plt.grid()
    plt.show()
```
下面就来说一下，这句`weights = weights + alpha * dataMatrix.T * error`梯度上升的更新算法的细节。  
## 2 梯度上升算法  
在吴恩达老师的机器学习公开课中，逻辑回归这里使用的是态度下降算法，来求得代价函数的最小值，来达到最小化代价的目的。
但是他并没有解释cost函数是从何而来，当时我以为是某位数学家巧妙设计而来，后来发现其实是由极大似然估计得来的。细节如下：  
![0](http://7xwp22.com1.z0.glb.clouddn.com/markdown/1476359015912.png)  
## 3 在线学习回归函数  
随机梯度上升算法与梯度上升算法的效果相当，但占用更少的计算资源。因为其一次用一个`sample`的特点，所以它还是一个在线算法，可以在新数据到来时进行一次参数更新，而不需要重新读取整个数据集来进行批处理运算。

1. 随机选取样本来进行更新参数，可以减少参数的周期性波动。
2. 学习率`alpha`非严格递减，可以在快要收敛的时候，放慢脚步，找到更加精确的收敛值。

```py
def stocGradAscent1(dataMatrix, classLabels, Cycles_100times=200):
    import random
    
    dataMatrix = np.array(dataMatrix)
    m,n =np.shape(dataMatrix)
    weights = np.ones(n)
    
    for j in range(Cycles_100times):
        randIndex=random.sample(range(m),m)
        
        for i in range(m):
            alpha = 0.01 + 4/(i+j+1)
            rand_i=randIndex[i]
            h = sigmoid(sum(dataMatrix[rand_i]*weights))
            error = classLabels[rand_i] - h
            weights = weights + alpha * error * dataMatrix[rand_i]
            
    return weights
```