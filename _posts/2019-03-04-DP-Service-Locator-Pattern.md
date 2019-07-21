---
layout: post
title: "Service Locator Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 服务定位器模式  

帮助clients找到远端服务器的服务。

#### 创建类模式  

- 抽象工厂  
- 建造者  
- 工厂方法  
- 原型  
- 单例

#### 使用场景  

- logging  
  - 使用贯穿整个app
  - 独立于任何业务逻辑  
  - 容易为各app的简单需求独立实现  
  - 常常基于环境或者部署的需求而改变

program在运行启动时用ServiceLocator注册所需的logger, ServiceLocator里静态的各种方法就可以在别处GetService<ILog>获得想要的logger。具体的logger类实现`ILog`的各种接口方法。

![servicelocator](/assets/gallery/servicelocator.png)   

#### 相关模式  

- 观察者模式：定位新的服务，在他们变得可用时
- 代理模式：总可以用一个服务定位器来讲代理和实现匹配  
- 适配器模式：适配器模式可以将具体services适配到符合通用接口  
- 依赖注入：在创建客户端时传入services

<!--more-->  