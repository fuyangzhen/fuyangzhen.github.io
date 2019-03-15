---
layout: post
title: "Strategy Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 策略模式  

策略模式就是在多种不同业务逻辑上抽象出策略接口和context类，context类的构造函数接收该接口的参数，其余函数用来封装策略的函数。然后在client端实例化具体的策略类，传给context实例化，然后用context对象的函数来使用传入的策略的函数。

1. **需要在不同时间应用不同的业务规则（算法），就可以考虑使用策略模式处理这种变化。**   

2. *switch*可能是一个信号表明此处可以考虑使用策略模式，即使用接口来代替switch。
3. 简化了单元测试，每个算法都有自己的类，可以通过自己的接口单独测试。  

4. 再抽象一层意味着，客户端（使用端）代码可以直接用更上层的context，就可以在需要修改时不用更改主体logic，是一种解耦，避免大量修改代码。  
5. 变体：若允许，可简化策略类为函数，用delegate传入context的参数来使用这些策略函数。  
6. （大化设计模式）Client端用switch来选择策略传给context时，可以让这个switch转到context的构造函数，这样就把策略的选择逻辑与客户端分离开，耦合更加低。  

![strategy](/assets/gallery/strategy.png)    





<!--more-->  