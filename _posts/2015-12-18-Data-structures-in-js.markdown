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

第三章 Lists


