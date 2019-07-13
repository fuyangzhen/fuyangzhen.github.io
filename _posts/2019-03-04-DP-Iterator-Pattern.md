---
layout: post
title: "Iterator Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 迭代器模式  

当你需要遍历访问聚集对象，可以有多种遍历方式时，应该考虑使用迭代器模式。

迭代器模式，分离了集合对象的遍历行为，抽象出一个迭代器类来负责遍历， 这样即可以做到不暴露集合内部结构，又可以让外部代码透明地访问集合内部数据。

aggregate（collection）负责收集聚合数据，然后交给iterator去操作遍历。

![iterator](/assets/gallery/iterator.png)    

####  .NET的自带接口  

实现了这两个接口IEnumerator & IEnumerable，就可以使用foreach关键字来遍历集合对象。  

```C#  
public interface IEnumerator
{
    object Current
    {
        get;
    }
    bool MoveNext();
    void Reset();
}

public interface Ienumerable
{
    IEnumerator GetEnumerator();
}
```

#### 相关模式    

- 工厂模式：
  - 迭代器可以通过aggregate中的工厂方法创建  
  - 多态迭代器使用工厂方法实例化适当的迭代器子类
- 组合模式：
  - 迭代器可被用来递归遍历组合结构，如树。

<!--more-->  