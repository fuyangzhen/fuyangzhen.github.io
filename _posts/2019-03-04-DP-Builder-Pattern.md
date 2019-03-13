---
layout: post
title: "Builder Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 建造者模式  

为了构造很复杂的对象，把logic从data中分离出来，以便能够重用logic。  

这些对象内部构建间的建造顺序通常是稳定的，但对象内部的构建通常面临着复杂的变化。  

做三明治的步骤顺序一般固定，但是每一步骤都可以有很多种变换。

![Builder-Pattern](/assets/gallery/Builder-Pattern.png)    

1. Director负责组织Concrete Builder的product构造逻辑顺序 (logic)；
2. Builder (abstract interface) 负责定义product的每一步抽象逻辑；
3. Concrete Builder定义product的每一步具体逻辑；



<!--more-->  