---
layout: post
title: "Mediator Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 中介模式  

*用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互作用，从而使其耦合松散，而且可以独立地改变他们间的交互。*   

##### 优点  

Mediator的出现减少了各个Colleague的耦合，使得可以独立地改变和复用各个Colleague类和Mediator。

把对象如何协作进行了抽象，将中介作为一个独立的概念封装在对象中，关注的对象就从对象各自本身的行为转义到他们之间的交互上来，是从一个更为宏观的角度看待系统。

##### 缺点  

当系统出现‘多对多’交互复杂的对象群时，不要急于使用中介者模式，而是烦死系统在设计上是否合理。

由于ConcreteMediator控制了集中化，浴室就把交互复杂性转变为了中介者复杂性，这就使ConcreteMediator比任何一个ConcreteColleague都复杂。

![Builder-Pattern](/assets/gallery/mediator.png)    

<!--more-->  