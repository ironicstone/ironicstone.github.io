---
layout: post
title:  《JS: The good parts》读书笔记
date:   2015-3-9 11:27:16
summary:
categories: javascript
tags: 读书笔记 js
---

## Javascript: The good parts

### Chapter 1: Good parts

{% highlight javascript %}
// A 'method' method is used to define new methods.

Function.prototype.method = function(name,func) {
    this.prototype[method] = func;
    return this;
}
{% endhighlight %}

### Chapter2: Grammer

- 注释时，建议使用//而非/* */,因为在js正则表达式中可能会出现*/字符，示例如下

{% highlight javascript %}

/*
    var rm_a = /a*/.match(s);
 */

{% endhighlight %}

- Javascript只有一个单一的数字类型，它在内部被表示为64位的浮点数，和java中的double一样，它没有分离出整数类型，所以1和1.0是相同的值

- 值NaN是一个数值，它表示一个不能产生正常结果的的运算结果，NaN不等于任何值，包括它自己，可以使用isNaN(number)检测NaN

- JS没有字符类型，表现为一个字符的字符串，\是转义字符

- 下面的值被当作假
    - false
    - null
    - undefined
    - 空字符串''
    - 数字0
    - 数字NaN

typeof运算符产生的值有number,string,boolean,undefined,function,object.如果运算数是一个数组或者null，那么结果是object。

