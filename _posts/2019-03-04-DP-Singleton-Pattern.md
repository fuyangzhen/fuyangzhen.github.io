---
layout: post
title: "Singleton Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 单例模式  

##### 影响  

1. 单例为相关使用它的类引入了紧密的耦合；
2. 为测试带来了困难；
3. 通常使用IOC容器来避免耦合和测试问题；
4. 应该用IOC容器来管理对象声明周期。

#### 线程不安全单例

##### 普通单例  

1. 直接在单例上调用起内部方法；
2. 可以将单例赋值给变量；  
3. 可以直接当参数传进其他方法；
4. **线程不安全**，不能用于多线程，如ASP.NET。

![singleton](/assets/gallery/singleton.png)   

#### 线程安全单例  

##### 单锁

![singleLock](/assets/gallery/singleLock.png)  

##### 双锁  

![doubleLock](/assets/gallery/doubleLock.png)  

##### 惰性单例  

1. 线程安全的惰性单例模式，其中用到的是嵌套类的静态构造函数。
2. **静态构造函数是属于类的，而不是属于实例的，只会在创建第一个静态实例或引用其中的静态成员之前，由.NET自动调用，且只被调用一次。**
3. **静态构造函数没有访问修饰符，也没有参数。如果没有写静态构造函数，而类中包含带有初始值设定的静态成员，那么编译器会自动生成默认的静态构造函数。**  

![lazysingleton](/assets/gallery/lazysingleton.png)   

![anotherLazy](/assets/gallery/anotherLazy.png)

#### 关联的模式  

- 抽象工厂  
- 工厂方法  
- 建造者

### IOC Container  

向容器注册好所需的每个对象及其接口。对象的构造函数如果需要其他对象，也一并注册好，然后容器在 resolve 所需的 object 时会自动的找到构造函数所需的参数，直接帮你构造好。

<!--more-->  