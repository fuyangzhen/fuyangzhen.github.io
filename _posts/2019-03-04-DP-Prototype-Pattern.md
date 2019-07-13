---
layout: post
title: "Prototype Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 原型模式  

用原型实例指定创建对象的种类，并通过拷贝这些原型创建新的对象。  

一般在初始化的信息不发生变化的情况下，克隆是最好的办法。既可以隐藏对象创建的细节，又对性能大大提提高。

- memento对象只负责存储状态（value objects）。
- caretaker负责管理memento堆栈，保存originator的状态以及供其恢复堆栈内存储的状态。 
- 只有originator可以创建或恢复它的状态。

把数据结构和对其的操作解耦，使得操作可以自由地添加。  

ObjectStructure枚举需要被访问者操作的元素，枚举时每个element accept visitor就是在传入visitor并使其在其中调用逻辑方法。

![prototype.png](/assets/gallery/prototype.png)  

#### 场景示例  

1. 构造函数实例化对象代价较高  
2. 对象内部各状态都很重要  
3. 隐藏了构造函数（internal constructor）

**注意**  

1. this.MemberwiseClone()是浅拷贝（非静态字段），引用类型只能拷贝引用。
2. 如果要深拷贝，可以先将引用类型参数对象的类实现C#Icloneable接口，然后在引用它的对象的Clone()方法里将它clone出来，然后再一个个复制其余值类型。

<!--more-->  

- 

  