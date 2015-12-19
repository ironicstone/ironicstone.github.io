---
layout: post
title:  《Data Structures and Algorithm with Javascript》读书笔记
date:   2015-12-18 21:34:02
summary: 读书笔记
categories: algorithms
tags: js algorithms book
---
## 数据结构和算法---Javascript描述

### 第一章

介绍javascript运行环境以及js编程的基本语法，可以跳过。

---

### 第二章 数组

在js中数组其实是一种特殊的javascript对象，index就是对应的属性值（key）。当数字作为索引时，在内部会将其转化为字符串以作为属性值(key)使用。

__Using Arrays__

{% highlight javascript %}
// Creating Arrays

var numbers = [];
var numbers = [1,2,3,4]; // length = 4

var numbers = new Array();
var numbers = new Array(1,2,3,4);  // length = 4
var numbers = new Array(10)   // length = 10

// Different data type
var objects = [1,'joe',true,null];

// Verify an array
Array.isArray(number);
Array.isArray(objects);
{% endhighlight %}

数组直接赋值是一个浅拷贝，相当于一个引用。

__Accessor Functions__

- searching for value
    - [array].indexOf([str])  // return the index of the first occurrence
    - [array].lastIndexOf([str])   // return the index of the last occurrence
- string representations of arrays
    - [array].join([symbols])
- creating new arrays from existing arrays
    - [array].concat([array])  // put two or more arrays together
    - [array].splice([starting position],[number of elements]) // it will change the original array

{% highlight javascript %}
// splice function can do anything to access the array, return the extract ones.
var itDiv = ['a','b','c','d','e'];
var dmpDept = itDiv.splice(2,2);
var cisDept = itDiv;
console.log(dmpDept); // 'c','d'
console.log(cisDept); // 'a','b','e'

var numbers = [0,1,2,3,4];
numbers.splice(0,1); // delete items,set the number of elements to 1
numbers.splice(0,0,0,1) // insert values to the array, set the number of elements to 0
{% endhighlight %}

__Mutator Functions__

- Adding Elements to an Array--> push(),unshift()
- Removing Elements from an Array --> pop(),shift()
- Adding and Removing Elements from the Middle of an Array --> splice([],[],newElements)
- Putting Array Elements in Order --> reverse(),sort(func())

__Iterator Functions__

Non-Array-Generating Iterator Functions

- forEach(func)
- every(func), applies a Boolean function to an array and returns true if the function can return true for every element in the array
- some(func), applies a Boolean function to an array and return true if at least one of the elements in the array meets the crierion of the Boolean function.
- reduce(func)
- reduceRight()

Iterator Functions That Return a New Array

- map()
- filter()

__Two-Dimensional and Multidimensional Array__

{% highlight javascript %}
Array.matrix = function(numrows, numcols, initial) {
    var arr = [];
    for(var i=0; i < numrows; i++) {
    var columns = [];
    for(var j=0; j < numcols; j++) {
        columns[j] = initial;
    }
    arr[i] = columns;
    }
    return arr;
}
{% endhighlight %}

Javascript can handle jagged array.

__Exercises__

{% highlight javascript %}
// 第一题
function grades() {
    this.data = [];
    this.add = add;
    this.average = average;
}

function add(num) {
    this.data.push(num);
}

function average() {
    var sum = 0;
    for(var i=0; i < this.data.length; i++) {
        sum += this.data[i];
}
    return sum/this.data.length;
}

//第四题
function letters() {
    this.data = [];
    this.add = add;
    this.toWord = toWord;
    function add(letter) {
    this.data.push(letter);
}
    function toWord() {
    return this.data.join('');
}
}

{% endhighlight %}

---

### 第三章 Lists

列表是一种最自然的数据组织形式，如果数据存储的顺序不重要，也不必对数据进行查找，那么列表就是一种再好不过的数据结构。

---

### 第四章 栈

后进先出（LIFO，last-in-first-out）

__对栈的操作__

- push()
- pop()
- peek()

{% highlight javascript %}
// 定义一个栈的类
function Stack() {
    // 属性
    this.dataStore = [];
    this.top = 0;
    // 方法
    this.push = push;
    this.pop = pop;
    this.peek = peek;
    this.clear = clear;
    this.length = length;
}

function push(element) {
    this.dataStore[this.top++] = element;
}

// 弹出并返回栈顶的值
function pop() {
    return this.dataStore[--this.top];
}

function length() {
    return this.top;
}

function peek() {
    return this.dataStore[this.top-1];
}

function clear() {
    this.top = 0;
}

{% endhighlight %}

——使用stack类——

几个典型的应用场景

①数制间的互相转换

将数字n转化为以b为基数的数字，步骤如下:

1. 最高位为n%b,并压栈
2. 使用n/b代替n
3. 重复步骤1和2，直到n等于0，且没有余数。
4. 持续将栈内元素弹出，直到栈空，依次将这些元素排列，就可以得到最终结果。

{% highlight javascript %}
function mulBase(num,base) {
    var s = new Stack();
    do {
    s.push(num%base);
    num = Math.floor(num /= base);
}while(num>)
while(s.length()>0) {
    converted += s.pop();
}
return converted;
}

var converted = parseInt(num,base);  // 快速计算
var converted = num.toString(base);  // 快速转换
{% endhighlight %}

②回文

思路：将每个字符按顺序压栈，当字符入栈之后弹出，就得到一个反转后的字符串，接下来与原字符串比较即可。

{% highlight javascript %}
function isPalindrome(word) {
    var s = new Stack();
    for(var i = 0; i < word.length; i++) {
    s.push(word.getCharAt(i));
    }
    var rword = '';
    while(s.length() > 0) {
    rword ++ s.pop();
    }

    if(word===rword) {
        return true;
    } else {
        return false;
    }
}

// 语法糖部分
return word===word.split('').reverse().join('');
{% endhighlight %}



