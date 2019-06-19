---
layout: post
title: "Template Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 模板模式  



定义一个操作中的算法股价，将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

*即上升子类中重复代码到基类中*

*复杂的状态转换逻辑被分散到各个具体子状态对象中，然后根据context的具体需求，context上绑定的状态为其交接其他状态*

将特定的状态相关的行为都放入一个对象内，由于所有与状态相关的代码都存在于某个ConcreteState内，所以通过定义新的子类就可以很容易地增加新的状态和转换。

![template](/assets/gallery/template.png)    

#### 应用场景  

要完成在某一细节层次一致的一个过程或一系列步骤，但其个别步骤在更详细的层次上实现可能不同时，我们通常考虑用模板方法模式处理。  

<!--more-->  

#### Hooks

钩子是在抽象基类中声明但没实现的方法。

子类可以以不同的角度勾过来算法的行为，或者可以直接忽略钩子。

#### Hollywood 原则  

- “Don't call us, we'll call you.”
- 高层组件不应该依赖低层组件  
- 模板方法模式中的基类属于高层组件，客户端应该依赖它们
- 而其子类属于低层实现—它们自己不调用任何东西，而仅仅被高层模板方法调用

#### 使用影响  

- 算法步骤必须被知道，并且在使用时相对缺乏弹性。
- 依赖继承，而不是去组合，这可能使其使用受到一定限制。**可以用策略模式去组合**。
- 单个继承很难将两个子算法合并成一个。**可以使用装饰模式**。

#### 相关模式  

- 策略模式：将完整算法实现注入到另外的模块中。
- 装饰模式：从一些子部件组成算法或行为。
- 工厂方法模式：为创建类型的新实例去定义一个通用接口。


