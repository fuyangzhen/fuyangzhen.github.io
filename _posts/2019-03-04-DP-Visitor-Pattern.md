---
layout: post
title: "Visotor Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 访问者模式  

visitor是一个作用于某对象结构中各元素的操作。该模式可以在不改变各元素的前提下定义作用于这些元素的新操作。  

把数据结构和对其的操作解耦，使得操作可以自由地添加。  

ObjectStructure枚举需要被访问者操作的元素，枚举时每个element accept visitor就是在传入visitor并使其在其中调用逻辑方法。

![visitor](/assets/gallery/visitor.png)    

<!--more-->  