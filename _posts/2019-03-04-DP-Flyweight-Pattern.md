---
layout: post
title: "Flyweight Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 享元模式  

用共享技术有效地支持大量细粒度的对象。

#### 使用场景  

1. 程序使用了大量的对象，造成了很大的存储开销；
2. 对象的大多数状态可以实现为外部状态（作为参数），这样可以从类中抽出外部状态，用相对较少的共享对象取代很多组对象（棋子的类型为内部状态，所在棋盘的位置为外部状态）

#### 目的  

1.  减少大量细粒度对象存储开销；
2. 同时共享要在多个上下文中使用的对象；
3. 保持面向对象的粒度和灵活性；

#### 关联的模式  

- 组合模式（叶节点可被实现为享元）
- 策略模式（算法可被实现为享元）

- 状态模式

![flyweight](/assets/gallery/flyweight.png) 

<!--more-->  