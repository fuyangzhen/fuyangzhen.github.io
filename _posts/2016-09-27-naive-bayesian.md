---
layout: post
title: "Naive bayesian"
tags: [ml, math]
comments: true
date: 2016-09-27 00:00:00
---
## 1 简介  
>朴素贝叶斯是一种构建分类器的简单方法。该分类器模型会给问题实例分配用特征值表示的类标签，类标签取自有限集合。它不是训练这种分类器的单一算法，而是一系列基于相同原理的算法：所有朴素贝叶斯分类器都假定样本每个特征与其他特征都不相关。举个例子，如果一种水果其具有红，圆，直径大概3英寸等特征，该水果可以被判定为是苹果。尽管这些特征相互依赖或者有些特征由其他特征决定，然而朴素贝叶斯分类器认为这些属性在判定该水果是否为苹果的概率分布上独立的。

>对于某些类型的概率模型，在监督式学习的样本集中能获取得非常好的分类效果。在许多实际应用中，朴素贝叶斯模型参数估计使用最大似然估计方法；换而言之，在不用到贝叶斯概率或者任何贝叶斯模型的情况下，朴素贝叶斯模型也能奏效。

>尽管是带着这些朴素思想和过于简单化的假设，但朴素贝叶斯分类器在很多复杂的现实情形中仍能够取得相当好的效果。2004年，[一篇分析贝叶斯分类器问题的文章](http://www.cs.unb.ca/~hzhang/publications/FLAIRS04ZhangH.pdf)揭示了朴素贝叶斯分类器取得看上去不可思议的分类效果的若干理论上的原因。尽管如此，2006年有[一篇文章](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.122.5901&rep=rep1&type=pdfPDF)详细比较了各种分类方法，发现更新的方法（如提升树和随机森林）的性能超过了贝叶斯分类器。

>朴素贝叶斯分类器的一个优势在于只需要根据少量的训练数据估计出必要的参数（变量的均值和方差）。由于变量独立假设，只需要估计各个变量的方法，而不需要确定整个协方差矩阵。  

以上来自[wiki百科](https://zh.wikipedia.org/wiki/朴素贝叶斯分类器)。  

<!--more-->  
  
## 2 朴素贝叶斯概率模型  
理论上，概率模型分类器是一个条件概率模型：  
 
![1](https://wikimedia.org/api/rest_v1/media/math/render/svg/28079310be466655aad2c65f245e4cb412e8b117)  
  
独立的类别变量`C`有若干类别，条件依赖于若干特征变量 
`F1,F2,...,Fn`。但问题在于如果特征数量`n`较大或者每个特征能取大量值时，基于概率模型列出概率表变得不现实。所以我们修改这个模型使之变得可行。 贝叶斯定理有以下式子：  

![2](https://wikimedia.org/api/rest_v1/media/math/render/svg/f2c8595ffd1c98706f679d2586ccb73c95336d71)  

用朴素的语言可以表达为：  

![3](https://wikimedia.org/api/rest_v1/media/math/render/svg/679e25db34f602d562e503af6d772125f78ab31e)  

实际中，我们只关心分式中的分子部分，因为分母不依赖于`C`而且特征`Fi`的值是给定的，于是分母可以认为是一个常数。这样分子就等价于联合分布模型：  
  
![4](https://wikimedia.org/api/rest_v1/media/math/render/svg/28079310be466655aad2c65f245e4cb412e8b117)  

重复使用链式法则，可将该式写成条件概率的形式，如下所示：  

![5](https://wikimedia.org/api/rest_v1/media/math/render/svg/28079310be466655aad2c65f245e4cb412e8b117)  
 ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/31845b172ec5c0b9b5ed916deaf929a14b7e2a87) 
![](https://wikimedia.org/api/rest_v1/media/math/render/svg/0be67f84d7213f91f3ea647923462aab194ab86d)   
![](https://wikimedia.org/api/rest_v1/media/math/render/svg/a474bd86779001186e9c6ba2b9b3801a81510efb)  
![](https://wikimedia.org/api/rest_v1/media/math/render/svg/1a76dc3801fdf20ea4ae680b7be6b301fceea193)  
![](https://wikimedia.org/api/rest_v1/media/math/render/svg/d7367ba741b4ca24e90dd690d2894e4a797a05e9)

现在“朴素”的条件独立假设开始发挥作用:假设每个特征`Fi,j!=i`是条件独立的。这就意味着：  

![6](https://wikimedia.org/api/rest_v1/media/math/render/svg/14ae825a0763ef510201f3f3bc22ae21728d3b6d)  

对于`i!=j`，所以联合分布模型可以表达为：  

![7](https://wikimedia.org/api/rest_v1/media/math/render/svg/4d98d85764ca413554badf4f63c43a0aaa7a7631)  

这意味着上述假设下，类变量`C`的条件分布可以表达为：  

![8](https://wikimedia.org/api/rest_v1/media/math/render/svg/7e2c830eac90468e5839385df574ae31d4fc1dbc)  

其中`Z`(证据因子)是一个只依赖于`F1,...,Fn`等的缩放因子，当特征变量的值已知时是一个常数。由于分解成所谓的类先验概率`p(C)`和独立概率分布`p(Fi|C)`，上述概率模型的可掌控性得到很大的提高。如果这是一个`k`分类问题，且每个`p(Fi|C=c)`可以表达为`r`个参数，于是相应的朴素贝叶斯模型有`(k−1) + nrk`个参数。实际应用中，通常取`k=2`（二分类问题），`r=1`(伯努利分布作为特征），因此模型的参数个数为`2n+1`，其中`n`是二值分类特征的个数。  
### 从概率模型中构造分类器  
讨论至此为止我们导出了独立分布特征模型，也就是朴素贝叶斯概率模型。朴素贝叶斯分类器包括了这种模型和相应的决策规则。一个普通的规则就是选出最有可能的那个：这就是大家熟知的最大后验概率（MAP）决策准则。相应的分类器便是如下定义的classify公式：  
![](https://wikimedia.org/api/rest_v1/media/math/render/svg/1f164389c02ead6cc42c72ab5821032961af8f01)  
即选取能使上式达到最大的`c`作为预测样本的类。  
## 3 代码实现  
朴素贝叶斯应用常见领域就是垃圾邮件识别，这里我们选取另一类似文字识别领域：征婚广告信息，来比较两个城市在广告用词上是否不同。若不同，他们各自的常用词又是哪些？  

利用`feedparse`RSS源解析器，来下载所需的相关文档。在`anaconda3-4.1.0`环境下用`conda install feedparser`命令来安装`feedparser`模块。 

可用下面的方式来获取文本信息： 

```py
ny=feedparser.parse('http://newyork.craigslist.org/stp/index.rss')
sf=feedparser.parse('http://sfbay.craigslist.org/stp/index.rss')
```

### 3.1 词典生成函数  
函数传入单词的list，然后返回用到的词的词典list：

```py
import operator
import re
import random
import feedparser
import numpy as np
from numpy import array

def createVocabList(dataSet):
    vocabSet=set([])
    for document in dataSet:
        vocabSet=vocabSet | set(document)
    return list(vocabSet)
```  
### 3.2 词频统计函数  

```py
def bagOfWords2VecMN(vocabList,inputSet):
    returnVec=[0]*len(vocabList)
    for word in inputSet:
        if word in vocabList:
            returnVec[vocabList.index(word)] += 1
#         else:
#             print('the word: %s is not in my Vocabulary!'%word)
    return returnVec
```  
### 3.3 朴素贝叶斯分类器训练函数  
```py
def trainNB0(trainMatrix,trainCategory):
    numTrainDocs=len(trainMatrix)            #输入文本的总个数
    numWords=len(trainMatrix[0])             #每个文本对应的词典总词数
    pAbusive=sum(trainCategory)/numTrainDocs #文本可能属于类别1的概率
    p0Num=np.ones(numWords)
    p1Num=np.ones(numWords)
    p0Denom=2
    p1Denom=2
    for i in range(numTrainDocs):
        if trainCategory[i]==1:              #将标记中属于类别1的拿出来统计
            p1Num += trainMatrix[i]          #问题文本中各个词出现的次数
            p1Denom += sum(trainMatrix[i])   #累加类别1文本的总词数
        else:
            p0Num += trainMatrix[i]
            p0Denom += sum(trainMatrix[i])
    p1Vect=np.log(p1Num/p1Denom)
    p0Vect=np.log(p0Num/p0Denom)
    return p0Vect,p1Vect,pAbusive
```  
### 3.4 分类函数  
利用训练好的分类器返回的结果，就可以基于朴素贝叶斯的概率准则来判断类别了，其中采用ln函数可以防止太多的概率小数累乘，导致计算结果下溢：

```py
def classifyNB(vec2Classify,p0Vec,p1Vec,pClass1):
    p1=sum(vec2Classify*p1Vec)+np.log(pClass1)    #ln(a*b)=ln(a)+ln(b)
    p0=sum(vec2Classify*p0Vec)+np.log(1-pClass1)
    if p1>p0:
        return 1
    else:
        return 0
```  
### 3.5 文本解析函数  
将从rss获取的文本信息，解析提取成单词的list：

```py
def textParse(bigString):
    #bigString=str(bigString)
    listOfTokens=re.split(r'\W*',bigString)  #注意大写的W是正则匹配任何非字母数字字符
    return [tok.lower() for tok in listOfTokens if len(tok)>2]
```  
留言中出现最多的代词，介词，冠词等组成了语言中冗余和结构辅助性内容，这对分类器的性能有着不小的影响，去掉这些词可使样本具有更高的代表性，特征更加突出：

```py
def calcMostFreq(vocabList,fullText):
    freqDict={}
    for token in vocabList:
        freqDict[token]=fullText.count(token)
    sortedFreq=sorted(freqDict.items(),key=operator.itemgetter(1),reverse=True)
    return sortedFreq[:30]
```  
### 3.6 操作封装函数  
最后写一个便利函数来封装所有操作。随机选取数据的80%用作训练集，剩下的用作测试集：

```py
def localWords(feed1,feed0):
    docList=[]; classList = []; fullText =[]
    minLen = min(len(feed1['entries']),len(feed0['entries']))
    for i in range(minLen):
        wordList = textParse(feed1['entries'][i]['summary'])
        docList.append(wordList)
        fullText.extend(wordList)
        classList.append(1) #NY is class 1
        wordList = textParse(feed0['entries'][i]['summary'])
        docList.append(wordList)
        fullText.extend(wordList)
        classList.append(0)
    vocabList = createVocabList(docList)#create vocabulary

    top30Words = calcMostFreq(vocabList,fullText)   #remove top 30 words
    print('top30Words:',top30Words)
    for word in top30Words:
        if word[0] in vocabList:
            vocabList.remove(word[0])
            
    trainingSet=range(2*minLen);testSet=[]
    randIndex=random.sample(range(2*minLen),2*minLen)
    testSet=randIndex[:20];trainingSet=randIndex[20:]
        
    trainMat=[];trainClasses=[]
    for docIndex in trainingSet:
        trainMat.append(bagOfWords2VecMN(vocabList,docList[docIndex]))
        trainClasses.append(classList[docIndex])
    p0V,p1V,pSpam=trainNB0(array(trainMat),array(trainClasses))
    errorCount=0
    for docIndex in testSet:
        wordVector=bagOfWords2VecMN(vocabList,docList[docIndex])
        if classifyNB(array(wordVector),p0V,p1V,pSpam)!=classList[docIndex]:
            errorCount+=1
    print('the error rate is: ',errorCount/len(testSet))
    return vocabList,p0V,p1V
```
### 3.7 最具代表性的词汇显示函数  
将两个城市杯提及最多的词汇找出来并排序，看看大家关注的地方有什么不同：

```py
def getTopWords(ny,sf):
    vocabList,p0V,p1V=localWords(ny,sf)
    topNY=[]; topSF=[]
    for i in range(len(p0V)):
        if p0V[i] > -4.8 : topSF.append((vocabList[i],p0V[i]))
        if p1V[i] > -4.8 : topNY.append((vocabList[i],p1V[i]))
    sortedSF = sorted(topSF, key=lambda pair: pair[1], reverse=True)
    print("--------------------------SF**SF**SF--------------------------")
    i=0
    for item in sortedSF:
        print('%12s'%item[0],end='');i+=1
        if i%5==0:
            print()
    sortedNY = sorted(topNY, key=lambda pair: pair[1], reverse=True)
    print("\n--------------------------NY**NY**NY--------------------------")
    i=0
    for item in sortedNY:
        print('%12s'%item[0],end='');i+=1
        if i%5==0:
            print()
```  
## 4 代码测试  
为了更精确地估计分类器的错误率，应该多次迭代后取平均值：

```py
ny=feedparser.parse('http://newyork.craigslist.org/stp/index.rss')
sf=feedparser.parse('http://sfbay.craigslist.org/stp/index.rss')

for i in range(10):
    localWords(ny,sf)

# 结果如下：
# the error rate is:  0.2
# the error rate is:  0.4
# the error rate is:  0.5
# the error rate is:  0.3
# the error rate is:  0.4
# the error rate is:  0.45
# the error rate is:  0.4
# the error rate is:  0.4
# the error rate is:  0.4
# the error rate is:  0.3
```  
这里得出的测试集错误率，能在一定程度上反映出两个城市的文本信息是否有显著差异。若错误率过大(接近0.5)，则说明差异不大；若错误率过小，则说明差异明显。然而这里的数据集规模太小（每个城市各25个），很可能导致随机选取的训练集不平衡，每次训练出来的词频list`p0V、p1V`也有很大差异，无法很好地代表每个城市发布的信息的特点，所以每次预测结果也和预期有很大不同。  

```py
getTopWords(vocabList,p0V,p1V)

# 结果如下：
# the error rate is:  0.45
# --------------------------SF**SF**SF--------------------------
#      married         get        here         age  friendship
#          don      things         one        lady     seeking
#         back         all        male         man      family
#         post    educated        cant     forward        also
#     lingerie        need   wondering        find       chill
#        happy       hello        well      please      artist
#        there     between        want        take     nothing
#         area      prefer       title       young        real
#       really      female   travelingrelationship        talk
#         love      fights        feel        when    believes
#         wish        says         his     company        ever
#      wearing        much       years    employed
# --------------------------NY**NY**NY--------------------------
#         nice        post      single         was        want
#         that      female        paul    recently   mccartney
#      sitting    favorite         get       buddy        fall
#          did        send     panties        whatconversation
#         here       enjoy     waiting       place     lesbian
#         host      mature        from         now         let
#        times         few        best
```