---
layout: post
title: "Bridge Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00
comments: true
---  

## 桥连模式  

abstraction -> abstraction  -> implementation

对象可多角度抽象出类别时，只用一种类别树来继承，耦合就太重了，会使类的定义大量增加，抽象和实现内在是一种依赖耦合关系。讲不同角度单独视为一类，不同角度的类通过聚合（桥）关联起来（参见大话设计模式），这也是一种抽象之上的再次抽象。

**再次抽象，是在扩展你的类的能力，将能力抽象为接口，解耦出来。这样就可为类实现各种其他的功能，而不用修改类，符合开闭原则。**

<!--more-->  

