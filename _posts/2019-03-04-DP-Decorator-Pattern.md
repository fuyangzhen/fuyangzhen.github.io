---
layout: post
title: "Decorator Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 装饰模式   

去超市买塑料袋，另外又用很多花哨的塑料袋来层层包装买的塑料袋。各个包装的先后顺序可控，按需选择装饰功能包装原始对象。

#### 目的

- 动态地向已有对象添加功能  
- 替代子类化  
- 弹性设计  
- 支持开闭原则

![decorator](/assets/gallery/decorator.png)    

*Decorator类构造函数传入Component的子类给属性赋值，该属性是protected的。*  

<!--more--> 

##### protected和private

- private是完全私有的，只有在类自己里面可以调用，在类的外部和子类都不能调用，子类也不能继承父类的private的属性和方法。

- protected虽然可以被外界看到，但外界却不能调用，只有自己及自己的子类可以调用（protected的属性和方法都可以被子类所继承和调用）。

- private和protected的共同点：外部都不可以访问。

- private和protected的不同点：在同一类中可视为一样，但在继承中就不同了，private在派生类中不可以被访问，而protected可以  

#### 使用场景  

- 遗留代码  
- 为UI控制元件添加功能  
- 为sealed类添加功能（如第三方dll 或者.net 框架）  

#### 利弊  

- 原始对象无法感知任何包装它的对象；
- 没有一个包含所有选项的功能丰富的类；
- 装饰对象能以混合搭配（各种顺序）的方式组合在一起;
- 把类中的装饰功能从类中搬移出去，只留下核心功能，简化原始类，去除重复的装饰逻辑。

#### 相关模式  

- 建造者模式

  建造者模式要求建造过程稳定不变，而装饰模式的包装过程不是固定的。