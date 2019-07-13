---
layout: post
title: "Unit of Work Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## 工作单元模式  

- 让业务逻辑远离数据访问代码
- 让业务逻辑远离追踪修改
- 允许业务逻辑和逻辑事务一起工作

迭代器模式，分离了集合对象的遍历行为，抽象出一个迭代器类来负责遍历， 这样即可以做到不暴露集合内部结构，又可以让外部代码透明地访问集合内部数据。

aggregate负责收集聚合数据，然后交给iterator去操作遍历。

####  .NET的自带接口  

UnitOfWork 连接起了Controllers与Repositorys,   Controllers（业务逻辑）只需要定义各种数据操作的逻辑，然后直接利用UnitOfWork  commit change就可以了。而无需关心底层的数据库连接，追踪修改。

```C#  
// IUnitOfWork 可供MVC里的Controllers作为构造函数参数使用
public interface IUnitOfWork{
    IRepository<Employee> Employees{get;}
    IRepository<TimeCard> TimeCards{get;}
    void Commit();
}

public class SqlUnitOfWork: IUnitOfWork{
    public SqlUnitOfWork(){
        // 连接数据库 _context
    }
    
    public IRepository<Employee> Employees{
        get{
            if (_employees == null){
                _employees = new SqlRepository<Employee>(_context);
            }
            return _employees;
        }
    }
    
    public void Commit(){
        _context.SaveChanges();
    }
}

public class SqlRepository<T>: ISqlRepository<T> where T: class, Ientity{
    public SqlRepository(ObjectContext context){
        _objectSet = context.CreateObjectSet<T>();
    }
    
    public void Add(T newEntity){
        _objectSet.AddObject(newEntity);
    }
    
    // other DB operations
}
```

#### 相关模式  

- 仓库模式

<!--more-->  