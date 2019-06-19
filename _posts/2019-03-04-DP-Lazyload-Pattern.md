---
layout: post
title: "Lazy load Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## Lazyload模式  

##### 常见场景  

1. property初始化用Lazy<T>关键字去set，get方法通过Lazy<T>对象.value去真正赋上值；  
2. virtual proxy，就是利用1中的概念，在开销大的property真正被call的时候才去为其赋值；
3. value holder同理2；
4. ghost就是field不完全被赋值状态的对象，一旦被调用其中的某个field就：该load上value的就set上value，暂时还没必要的就set上Lazy<T>。

<!--more-->  

##### 总结  

- 运用lazyload可以减少非必要数据的读取调用  
- 要避免过早优化，仅在有性能需求是使用lazyload

##### 相关模式  

- proxy ：virtual proxy
- strategy：ValueHolder
- template method : Ghost implementation