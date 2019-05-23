---
layout: post
title: "Observer Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 观察者模式  

#### 特征  

- 系统被分割成相互协作的很多类，但我们不希望为了维护相关对象间的一致性，而去使各类紧密耦合。

- 观察者模式所做的工作是解除耦合，让耦合双方依赖抽象，而非具体，从而使得各自的变化都不会影响另一方的变化。
- 多抽象，少具体可以降低耦合。(**依赖倒转原则**)
- 具体观察者可能并不属于同一基类，所以可以使用抽象接口来统一他们，都能实现update方法就行。
- delegate可以看做是对函数的抽象，是函数的‘类’，委托的实例将代表一个具体函数。
- 委托所搭载的方法并不需要属于同一类，但方法接口参数要相同。

![observer](/assets/gallery/observer.png)    

<!--more-->  

#### 使用场景  

- 一个对象依赖另一个对象
- 改变一个对象需要同时改变其他对象  
- 改变一个对象的时候通知它并不了解的其他对象

#### 模式进化  

- 传统抽象方式（四人帮）（需要保证所有observer来自同一基类，在subject中维护一个observer列表）
- 委托方式（old .net）  因为无法控制subject的引用的observer的析构，所以容易发生内存泄漏问题
- IObservable<T>方式（new .net）

![IObserver](/assets/gallery/IObserver.png)    

#### 常见使用场景  

- GUI controls  
- data binding  
- network events
- file watcher  
- MVC pattern

#### 潜在问题  

- 委托方式可能导致GC不干净
- subject来自外部其他线程 (监控网络事件或磁盘文件)，导致主线程的observer的update动作可能会使用非我们创建的线程。**这时候需要手动管理创建update的线程。**

#### 相关模式  

- MVC模式（model -> subject, view -> observer）