---
layout: post
title: "Factory Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

### Tips  

*可以用 反射技术+config 来去除分支判断逻辑带来的耦合 :*  

```C#
static IAutoFactory LoadFactory()
{
    string factoryName = Properties.Settings.Default.AutoFactory;
    return (IAutoFactory)Assembly.GetExecutingAssembly().CreateInstance(factoryName);
}
```

### 动机

1. 不确定接口的哪个具体实现来返回  
2. 对象的创建和表达需要分开  
3. 很多if/switch 来决定创建哪个具体的class  

### 目的  

* 把对象的实际创建从对象创建的选择逻辑中分离出来  
* 添加新的类和功能（生产工厂的类，工厂类），而不打破开闭原则  
* 把对象选择存在程序之外（DB or config）

## 简单工厂模式  

把类的实例化放在工厂内部的选择逻辑里。调用类知道它需要的具体工厂，直接用工厂实例的函数来返回所需的产品实例，这样就去除了客户端和具体产品的依赖。

<!--more-->  

## 工厂方法模式  

比简单工厂多了一个抽象工厂接口和更多的专职工厂。工厂方法把一个类的实例化延迟到其子类。

工厂接口定义创建产品实例的方法，子工厂类实现这些方法。  

![factory1](/assets/gallery/factory1.png)

## 抽象工厂模式  

* 当不同类别的产品需要工厂来实现时，就用另一个角度来定义具体不同的工厂类：查询操作的接口可以有db1和db2两种不同的类，插入操作也是。这时就可以根据db1和db2来定义两种工厂类，在他们的定义里聚合其各类的实例化方法。 
* 客户端通过抽象接口操纵实例，产品的具体类名被具体工厂实现分离，不会出现在客户代码中。  
* 辅以反射和config来把分支判断将程序由编译时转为运行时。

![factory2](/assets/gallery/factory2.png)

### 相关创建型模式  

* 单例模式  
* [建造者模式](https://fuyangzhen.github.io/2019/03/04/DP-Builder-Pattern/)    
* 对象池  
* 原型模式

