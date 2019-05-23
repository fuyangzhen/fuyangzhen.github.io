---
layout: post
title: "Model View Presenter Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## Model View Presenter 模式  

view 和 presenter 相互绑定，view负责关联html内部的函数和变量。presenter 负责把view传给他的args和调用的model作为参数传入自己绑定的view 的函数中。

- view 直接和html关联交互
- presenter （aka controller）和view相互绑定，然后view在需要展示时传参调用presenter 的相关函数，这些函数再运用具体的逻辑去取需要的model传入view的函数去渲染展示
- model只负责为presenter 提供数据
- 这样就形成了很好的分离解耦 ：view只负责展示，presenter只负责业务逻辑，model只负责提供数据

#### 目的  

- 职责分离
  - 需要展示的数据 *Model*
  - 业务逻辑 *presenter*
  - 展示数据 *view*

- 使模式中这些合作者（MVP）能单独测试
- 比查找业务逻辑更具可读性和可维护性

![MVP](/assets/gallery/MVP.png)    

<!--more-->  

MVP模式作为UI layer，model是仅供view使用的数据的特殊用途view

![MVP2](/assets/gallery/MVP2.png)  



#### 相关模式  

- MVC模式
- MVVM模式
- Supervising Controller监督控制器
- Passive View被动式图