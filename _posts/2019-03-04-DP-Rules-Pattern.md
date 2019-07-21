---
layout: post
title: "Rules Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 规则模式  

*规则模式是一种特殊的命令模式*  

evaluator负责聚合评估规则，而每个具体规则只负责实现自己的规则和匹配。

![rule](/assets/gallery/rule.png)   

#### 使用场景

- 当系统内的条件判断复杂度上升，同样性质的增量改变是可预料的
- 系统会考虑选择哪些操作是适用的，并执行这些操作
- 系统需要支持用户创建的逻辑来决定何时以及如何应用操作

#### 业务规则引擎  

- 用于封装业务规则的软件系统  
- 通常支持业务用户编写规则  
- 存储在数据库或文件系统中的规则  
- .NET 里有 System.Workflow 包含 Rules Engine





<!--more-->  