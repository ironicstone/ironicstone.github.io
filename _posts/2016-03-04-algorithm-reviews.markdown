---
layout: post 
  
title:  算法笔记
date:   2016-3-4 10:06:37
summary:
categories: algorithms
tags: 算法 笔记
---

解决算法鲁棒性问题最好的方法是在动手写代码之前想好测试用例，考虑到边界条件、特殊输入（比如NULL指针，空字符串等）及错误处理。

<hr>

## 你不知道的Javascript

表面上看，Javascript并不具有块作用域的相关功能，但是存在以下几种情况：

- with:用with从对象创建的作用域仅在with声明中有效
- try/catch：catch分句会创建一个块级作用域，其中声明的变量仅在catch内部有效
- let

{% highlight javascript %}
// let实现块级作用域
{
  let a = 2
  console.log(a)  // 2
}

console.log(a)  // ReferenceError

// ES6之前使用try/catch实现

try {
  throw 2
} catch(a) {
  console.log(a)  // 2
}

console.log(a)  // ReferenceErroe

{% endhighlight %}

