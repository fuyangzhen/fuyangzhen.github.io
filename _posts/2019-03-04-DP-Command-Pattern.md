---
layout: post
title: "Command Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 命令模式  

#### 优点  

- 把命令的执行抽象为类，执行命令的client 与 命令逻辑及其依赖项 解耦；

- 收集命令为队列，使命令可以延迟执行;
- 命令，aka 事务（transaction），行动（action）;
- 可方便地对命令进行日志记录，验证，撤销；
- 很容易添加新命令，符合开闭原则；
- 完全自包含，客户端不会直接传任何参数给命令，而是通过一个invoker来调用命令；
- 注意为了安全要有个null command；

### 何时使用  

1. 当需要将执行命令的client与命令逻辑及其依赖项解耦时，请求操作的对象（invoker）与知道具体如何操作的对象（command & receiver ）分割开；
2. 设计命令行工具；
3. 真正需要实现命令的验证、撤销和恢复时。

#### 相关模式  

* 工厂模式，常用来结合到命令模式中，生产命令实例；
* 组合模式（执行一个命令，其所有子命令全部执行）。

![command](/assets/gallery/command.png)

<!--more-->  