---
layout: post
title: "Interpreter Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 解释器模式  

如果一种特定类型的问题发生频率足够高，那么可能就值得将该问题的各个实例表述为一个简单语言中的句子。

**当有一个语言(DSL)需要解释执行，并且你可以将该语言中的句子表示为一个抽象语法树时，可使用解释器模式。**设计迷你语言，解释迷你语言。但当文法非常复杂时，使用其他的技术如语法分析程序或编译器生成器来处理。  

***简单工厂加反射可以避免对switch case的修改，从而不用修改代码。***

![interpreter](/assets/gallery/interpreter.png)    

##### 相关模式  

组合模式

<!--more-->  