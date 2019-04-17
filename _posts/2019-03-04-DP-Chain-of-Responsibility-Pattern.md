---
layout: post
title: "Chain of Responsibility Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 责任链模式  

#### 何时使用  

* 对于一条message有超过一个handler可以处理它  
* 发送message的sender一开始并不能明确得知最终合适的handler  
* handler的集合可以被动态地定义（node handler用起来可以按需连接）  

#### 责任链优点  

* 减少耦合  
* 动态地管理 message handlers  
* 责任链的末尾可以适当的自定义  

#### 与其有关的模式  

* Composite组合模式  
* 责任树模式（也就是字面意思）

![strategy](/assets/gallery/chain-of-responsibility.png)    





<!--more-->  