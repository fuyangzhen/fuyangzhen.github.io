---
layout: post
title: "JS Note"
tags: [memo, other]
date: 2018-06-23 00:00:00
comments: true
---  

## Syntax Note

<!--more-->  

1. 循环体内部定义的var会传到外部，所以应该使用let来定义块作用域 
2. 字符前加上+会转为int（or float?）:  

```js
function char2Int(){
    let c = c;
    return +c;
};
```

3. ```
   // Array的sort()方法默认把所有元素先转换为String再排序：
   // 无法理解的结果:
   [10, 20, 1, 2].sort(); // [1, 10, 2, 20]
   ```

4. 返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量;

5. “创建一个匿名函数并立刻执行“(匿名函数必须加上“（）”才能立即使用)：  

   ```js
   (function (x) { return x * x }) (3);
   ```

6. 在返回的对象中，实现一个闭包，该闭包携带了局部变量`x`，并且，从外部代码根本无法访问到变量`x`。换句话说，**闭包就是携带状态的函数，并且它的状态可以完全对外隐藏起来**：  

   ```js
   'use strict';
   
   function create_counter(initial) {
       var x = initial || 0;
       return {
           inc: function () {
               x += 1;
               return x;
           }
       }
   }
   
   var c1 = create_counter();
   c1.inc(); // 1
   c1.inc(); // 2
   c1.inc(); // 3
   
   var c2 = create_counter(10);
   c2.inc(); // 11
   c2.inc(); // 12
   c2.inc(); // 13
   ```

7. `res += 1`等于`++res`;

8. 总结一下，有这么几条规则需要遵守：

   - 不要使用`new Number()`、`new Boolean()`、`new String()`创建包装对象；
   - 用`parseInt()`或`parseFloat()`来转换任意类型到`number`；
   - 用`String()`来转换任意类型到`string`，或者直接调用某个对象的`toString()`方法；
   - 通常不必把任意类型转换为`boolean`再判断，因为可以直接写`if (myVar) {...}`；
   - `typeof`操作符可以判断出`number`、`boolean`、`string`、`function`和`undefined`；
   - 判断`Array`要使用`Array.isArray(arr)`；
   - 判断`null`请使用`myVar === null`；
   - 判断某个全局变量是否存在用`typeof window.myVar === 'undefined'`；
   - 函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`。  

9. 没有`name`属性的`<input>`的数据不会被提交 ;

## React  

组件继承自react component class，一般分为三个属性：

1. 普通属性
2. state（component class预定义好的）自身的数据字段，应该代表着UI呈现状态在render中被使用
3. props（component class预定义好的）数据的接口

不要依赖当前的state和props计算下一个状态，因为他们的更新都是异步的。



**state的改变意味着渲染刷新**