---
layout: post
title: "MVP Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 代理模式  

#### 使用场景  

1. remote 远程代理。为一个对象在不同地址空间提供局部代表；
2. virtual 虚拟代理。对象创建的开销很大的时候，用代理去lazy loading；
3. protection 安全代理。控制真实对象的访问权限，即在访问一个对象时附加一些内务处理。

![proxy](/assets/gallery/proxy.png)   

 

![proxy2](/assets/gallery/proxy2.png)   