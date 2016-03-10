---
layout: post
title:  小笔记
date:   2016-1-4 10:06:37
summary:
categories: javascript
tags: 笔记
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


### Chapter 3 Object

Javascript的简单类型包括数字、字符串、布尔值、null和undefined。其他所有值都是对象。对象是属性的容器，每个属性都拥有名字和值，属性的名字可以使包括空字符串在内的任意字符串，如果属性名是一个合法的Javascript标识符且不是保留字，并不强制要求引号括住属性名。属性值可以使除undefined值之外的任何值。

Javascript包括一个原型链特性，允许对象继承另一个对象的属性。正确的使用能减少对象初始化的时间和内存消耗。

||运算符可以用来填充默认值，尝试检索一个undefined值将会导致TypeError异常，这可以通过&&运算符避免。

对象如果通过引用来传递，他们永远不会被拷贝。


for in 语句可以遍历一个对象中所有的属性名，使用for in 时属性名出现的顺序是不确定的，因此要对任何可能出现的顺序有所准备，如果要确保属性以特定的顺序出现，最好的办法就是完全避免使用for in 语句，而是创建一个数组，在其中包含正确属性的属性名序列。

删除对象的属性可能会让来自原型链中的属性浮现出来。

减少全局变量的污染，最小化使用全局变量的一个方法就是在你的应用中只创建唯一一个全局变量。只要把多个全局变量都整理在一个名称空间下，会显著降低与其他应用程序、组件或类库之间互相影响的可能性。


{% highlight javascript %}

var status = fligth.status||'unkown';   //取前值

flight.equipment  //undefined
flight.equipment.model  // throw 'TypeError'
flight.equipment && flight.equipment.model  //取后值 undefined

var a = {}, b = {}, c = {};   // a,b和c每个都引用一个不同的空对象

a = b = c = {};  // a,b和c都引用同一个空对象

// 使用typeof确定操作的类型

typeof flight.number

// 如果要剔除原型链中的属性，可以通过hasOwnProperty方法来验证

flight.hasOwnProperty('number')

var name;
for (name in another_stooge) {
    if (typeof another_stooge[name] !== 'function') {
    alert(name+ ':' + another_stooge[name]);
}
}


// 避免全局变量污染示例代码

var MYAPP = {};

MYAPP.stooge = {};
MYAPP.flight = {};

{% endhighlight %}


### Chapter 4 Functions

函数就是对象，每个函数对象在创建时也随带一个prototype属性，它的值是一个拥有constructor属性且值即为该函数的对象。函数的与众不同之处在于它们可以被调用。

调用函数时，每个函数接收两个附加的参数：this和arguments.

参数this在面向对象编程中十分重要，它的值取决于调用的模式。在javascript中一共有四种调用模式：方法调用模式，函数调用模式，构造器调用模式和apply调用模式，这些模式在如何初始化关键参数this上存在差异。

- 方法调用模式：函数作为一个对象的属性时称为一个方法，当方法被调用，this被绑定到对象上。方法可以使用this去访问对象，所以可以从对象中取值或修改对象。

- 函数调用模式：此模式下，this被绑定到全局对象。这是语言设计上的一个错误，理论上来说，当内部函数发生调用时，this应该仍然绑定在外部函数的this变量上。解决方法：给外部函数的this变量重新赋值，比如var that = this;

{% highlight javascript %}
obj.value = 1;
obj.double = function() {
    var that = this;
    var helper = function() {
    that.value = that.value + that.value;
};
    helper();
};

obj.double();

alert(obj.value);

{% endhighlight %}

- 构造器调用模式：结合new前缀调用的函数被称为构造器函数

{% highlight javascript %}
var T = function(string) {
    this.str = string;
};

T.prototype.get_str = function() {
    return this.str;
};

var myT = new T("hello");

alert(myT.get_str());


{% endhighlight %}

- Apply调用模式：apply方法让我们构建一个窜书数组去调用函数，同时也允许我们选择this的值，apply方法接受两个参数，第一个是绑定给this的值，第二个是参数数组。

{% highlight javascript %}

var temp = {
    str: "world"
};

var status = T.prototype.get_str.apply(temp);
alert(status);
{% endhighlight %}




参数arguments并不是一个真正的数组，它拥有一个length属性
