---
layout: post
title: "Model View  ViewModel Pattern"
tags: [Design Patterns]
date: 2019-03-04 00:00:00

comments: true
---  

## Model View  ViewModel模式  

需要依赖WPF/Silverlight来实现，即只适用于这两个框架。  

大体上和MVP类似：  

![MVVM](/assets/gallery/MVVM.png)   

1. Model: the data;
2. View: Binding to ViewModel set by the DataContext;
3. ViewModel: 
   - Exposes the Model as Properties or Commands
   - Must implement INotifyPropertyChanged

#### 优缺点  

优点：

- 减少code-Behind；
- Model不需要为了支持View去更改；
- 前端页面设计，和后端业务逻辑分离；
- 减少开发时间；
- Multi-targeting(project linking) : support .net multi-framework 

缺点：

- 创建了更多的文件；
- 简单的任务可能实现上也比较复杂；
- 缺少标准化；
- 特定地适用于WPF、Sliverlight平台；

#### 相关模式  

- Model View Presenter(MVP)
- Model View Controller(MCV)  
- Presentation Model(PM)