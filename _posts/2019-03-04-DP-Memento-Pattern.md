---
layout: post
title: "Memento Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 备忘录模式  

在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可以将该对象恢复到原先保存的状态。

- memento对象只负责存储状态（value objects）。
- caretaker负责管理memento堆栈，保存originator的状态以及供其恢复堆栈内存储的状态。 
- 只有originator可以创建或恢复它的状态。

把数据结构和对其的操作解耦，使得操作可以自由地添加。  

ObjectStructure枚举需要被访问者操作的元素，枚举时每个element accept visitor就是在传入visitor并使其在其中调用逻辑方法。

![memento](/assets/gallery/memento.png)

**注意**  

1. 相较于存储状态，一种变形是，存储反转操作。如：操作（+5），那就存储操作（-5）。
2. 有些场景的反转操作并不能得到之前的结果，如：图计算，翻译。

<!--more-->  

#### 相关模式  

- 命令模式： 命令可以用备忘录来提供撤销功能  

- 迭代器模式： 备忘录可以用来表示每次迭代的状态  

  