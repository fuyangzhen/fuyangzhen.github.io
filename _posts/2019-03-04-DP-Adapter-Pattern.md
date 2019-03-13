---
layout: post
title: "Adapter Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00
comments: true
---  

## 适配器模式  

client -> adaptor interface -> adaptor class -> adaptee

不要为了适应功能的interface而去继承修改，实现的耦合重不可复用，应该用adaptor来转换interface。

常见的一种简单adapter：继承自target类，在其定义中聚合adaptee，在adapter定义中override的target的方法中使用adaptee的方法。  （参见大话设计模式）

常见的另一种adapter：target类定义中直接聚合符合Iadapter的adapter实例，然后在target的方法中调用adapter实例的方法。这种方式是通过Iadapter接口来规范定义可接受的adapter实例方法。（参见pluralsight-patterns-library）   

<!--more-->    

### 与其有联系的其他模式  

1. 存储库模式  

2. 策略模式  

3. facade外观模式（涉及到包装简化，而适配器模式只是包装适配）  

   

### 原则  

初始设计的时候类的接口应该尽可能的规范统一，而不是使用适配器去调整。适配器适用于修补和完全不同环境的代码之间的适配。