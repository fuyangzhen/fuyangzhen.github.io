---
layout: post
title: "Null Object Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 空对象模式  

一种null检查的替代。为可能出现null的类单独实现一个null的子类，为其实现所需的方法。

**AKA** Stub, Active Nothing, Active Null   

**常被实现为一个单例**，没有随实例变化的行为或状态；

#### 目的  

1. 摆脱null检查的逻辑；
2. 提供无方法对象来代替null 引用；
3. 允许方法被null对象调用，null 引用就不行。  

#### 关联的模式  

- 单例模式
- 策略模式  
- 命令模式  
- 状态模式  
- others  

#### 优点  

- code更加干净简洁；
- 更少的null检查；
- 更少的分支，意味着更低的复杂度；
- 调用者无需关心到底得到的是真对象还是空对象；
- 必须让其他的开发人员知道某处实现了空对象。

![null](/assets/gallery/null.png) 

<!--more-->  