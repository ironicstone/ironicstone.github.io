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

---

第五章 队列

队列是一种列表，不同的是队列只能在<span class="red">队尾插入元素，在队首删除元素</span>。与生活中排队类似，先进先出。（FIFO,first-in-first-out）

__对队列的操作__

{% highlight javascript %}
function Queue() {
    this.dataStore = [];
    this.enqueue = enqueue;
    this.dequeue = dequeue;
    this.front = front;
    this.back = back;
    this.toString = toString;
    this.empty = empty;
}

function enqueue(element) {
    this.dataStore.push(elememt);
}

function dequeue() {
    this.dataStore.shift();
}

function front() {
    return this.dataStore[0];
}

function back() {
    return this.dataStore[this.dataStore.length - 1];
}

function toString() {
    var str = '';
    for(var i=0; i < this.dataStore.length; i++) {
    str += this.dataStore[i] + '/n';
}
    return str;
}

function empty() {
    if(this.dataStore.length===0) {
    return true;
} else {
    return false;
}
}
{% endhighlight %}

__使用队列__

基数排序（radix sort）时间复杂度 nlog(r)m,r为基数，m为堆数。

分为两类LSD(least significant digit，最低位)和MSD(most significant digit, 最高位)

[计数排序、桶排序和基数排序](http://blog.csdn.net/quietwave/article/details/8008572)

{% highlight javascript %}
// digit 表示位数
function distribute(nums,queues,n,digit) {
    for(var i=0; i < n; i++) {
    if(digit===1) {
    queues[nums[i]%10].enqueue(nums[i]);
} else {
    queues[nums[i]/10].enqueue(nums[i]);
}
}
}

function collect(queues,nums) {
    var i=0;
    for(var digit = 0; digit < 10; digit++) {
    while(!queues[digit].empty()) {
    num[i++] = queues[digit].dequeue();
}
}
}
{% endhighlight %}

<span class="blue">基于C语言的基数排序</span>

{% highlight c %}
#include<math.h>
testBS()
{
    inta[] = {2, 343, 342, 1, 123, 43, 4343, 433, 687, 654, 3};
    int *a_p = a;
    //计算数组长度
    intsize = sizeof(a) / sizeof(int);
    //基数排序
    bucketSort3(a_p, size);
    //打印排序后结果
    inti;
    for(i = 0; i < size; i++)
    {
        printf("%d\n", a[i]);
    }
    intt;
    scanf("%d", t);
}
//基数排序
voidbucketSort3(int *p, intn)
{
    //获取数组中的最大数
    intmaxNum = findMaxNum(p, n);
    //获取最大数的位数，次数也是再分配的次数。
    intloopTimes = getLoopTimes(maxNum);
    inti;
    //对每一位进行桶分配
    for(i = 1; i <= loopTimes; i++)
    {
        sort2(p, n, i);
    }
}
//获取数字的位数
intgetLoopTimes(intnum)
{
    intcount = 1;
    inttemp = num / 10;
    while(temp != 0)
    {
        count++;
        temp = temp / 10;
    }
    returncount;
}
//查询数组中的最大数
intfindMaxNum(int *p, intn)
{
    inti;
    intmax = 0;
    for(i = 0; i < n; i++)
    {
        if(*(p + i) > max)
        {
            max = *(p + i);
        }
    }
    returnmax;
}
//将数字分配到各自的桶中，然后按照桶的顺序输出排序结果
voidsort2(int *p, intn, intloop)
{
    //建立一组桶此处的20是预设的根据实际数情况修改
    intbuckets[10][20] = {};
    //求桶的index的除数
    //如798个位桶index=(798/1)%10=8
    //十位桶index=(798/10)%10=9
    //百位桶index=(798/100)%10=7
    //tempNum为上式中的1、10、100
    inttempNum = (int)pow(10, loop - 1);
    inti, j;
    for(i = 0; i < n; i++)
    {
        introw_index = (*(p + i) / tempNum) % 10;
        for(j = 0; j < 20; j++)
        {
            if(buckets[row_index][j] == NULL)
            {
                buckets[row_index][j] = *(p + i);
                break;
            }
        }
    }
    //将桶中的数，倒回到原有数组中
    intk = 0;
    for(i = 0; i < 10; i++)
    {
        for(j = 0; j < 20; j++)
        {
            if(buckets[i][j] != NULL)
            {
                *(p + k) = buckets[i][j];
                buckets[i][j] = NULL;
                k++;
            }
        }
    }
}
{% endhighlight %}

优先队列

当优先队列删除元素时，需要考虑优先权的限制。因此在存储对象时，可以定义一个优先码。重写dequeue函数。

{% highlight javascript %}
function dequeue() {
    var priority = this.dataStore[0].code;
    for(var i=0; i < this.dataStore.length; i++) {
    if(this.dataStore[i].code < priority) {
    priority = i;
}
}
return this.dataStore.splice(priority,1);
}
{% endhighlight %}

---

### 第六章 链表

数组不总是组织数据的最佳数据结构，因为很多编程语言中，数组的长度是固定的，而且添加和删除数据也很麻烦，因为需要将数组的其他元素向前或者向后平移。然后js中不存在这种问题，因为它们被实现成了对象，与其他语言的数组相比，效率很低。

{% highlight javascript %}
// 双向链表
function Node(element) {
    this.element = element;
    this.previous = null;
    this.next = null;
}

function LList() {
    this.head = new Node('head');
    this.find = find;
    this.insert = insert;
    this.display = display;
    this.remove = remove;
    this.findLast = findLast;
    this.dispReverse = dispReverse;
}

function find(item) {
    var currNode = this.head;
    while (currNode.element!=item) {
        currNode = currNode.next;
    }
    return currNode;
}

function insert(element,item) {
    var newNode = new Node(element);
    var currNode = this.find(item);
    newNode.next = currNode.next;
    newNode.previous = currNode;
    currNode.next = newNode;
}

function display() {
    var currNode = this.head;
    while(currNode.next!=null) {
        console.log(currNode.element);
        currNode = currNode.next;
    }
}

function findLast() {
    var currNode = this.head;
    while(currNode.next!=null) {
        currNode = currNode.next;
    }
    return currNode;
}

function dispReverse() {
    var currNode = this.findLast();
    while(currNode.previous!=null) {
        console.log(currNode.element);
        currNode = currNode.previous;
    }
}

function remove(item) {
    var currNode = this.find(item);
    if(currNode.next!=null) {
        currNode.previous.next = currNode.next;
        currNode.next.previous = currNode.previous;
        currNode.next = null;
        currNode.previous = null;
    }
}

{% endhighlight %}

---

### 第十章 树

定义：树由一组以边连接的节点组成，常见的组织结构图就是一种树的结构

常见术语：一棵树最上面的节点称为根节点，如果一个节点下面链接多个节点，那么该节点称为父节点，它下面的节点称为子节点。没有任何子节点的称为叶子节点。树可以分为几个层次，根节点是第0层，它的子节点是第一层，以此类推。规定空树的深度为-1，则深度为K的二叉树最大的节点数为2^(k+1)-1,同样可以计算得到，具有n个节点的完全二叉树的深度为[lg2(n+1)] - 1

左节点，右节点

__实现二叉查找树__

{% highlight javascript %}
function Node(data,left,right) {
    this.data = data;
    this.left = left;  // 左节点
    this.right = right;  // 右节点
    this.show = show;
}

function show() {
    return this.data;
}

// 二叉查找树 BST
function BST() { // Initialize
    this.root = null;
    this.insert = insert;
    this.inOrder = inOrder;
}

function insert(data) { // Insertion
    var n = new Node(data, null, null);
    if (!this.root) {
        this.root = n;
    } else {
        var current = this.root;
        var parent;
        while (true) {
            parent = current;
            if (data < current.data) {
                current = current.left;
                if (current == null) {
                    parent.left = n;
                    break;
                } else {
                    current = current.right;
                    if (current == null) {
                        parent.right = n;
                        break;
                    }
                }
            }
        }
    }
}

// 中序遍历
function inOrder(node) {
    if(!(node===null)) {
        inOrder(node.left);
        console.log(node.show() + ' ');
        inOrder(node.right);
    }
}

// 前序遍历
function preOrder(node) {
    if(!node) {
        console.log(node.show() + ' ');
        preOrder(node.left);
        preOrder(node.right);
    }
}

// 后序遍历
function postOrder(node) {
    if(!node) {
        postOrder(node.left);
        postOrder(node.right);
        console.log(node.show() + ' ');
    }
}

// 查找最大值和最小值
function getMin(node) {
    var current = node;
    while(!(current.left==null)) {
        current = current.left;
    }
    return current.data;
}

function getMax(node) {
    var current = node;
    while(!current.right) {
        current = current.right;
    }
    return current.data;
}

// 查找节点
function find(data) {
    var current = this.root;
    while(!current) {
        if (current.data == data) {
            return current;
        } else if (data < current.data) {
            current = current.left;
        } else {
            current = current.right;
        }
    }
}

// 删除节点
function remove(data) {
    root = removeNode(this.root,data);
}

// 分为三种情况，叶子节点直接删除，有一个子节点直接替换，
// 有两个子节点，本文的方法是用右子树中的最小节点替换当前节点。

function removeNode(node,data) {
    if (node == null) {
        return null;
    }
    if (data == node.data) {
        if (node.left==null&&node.right==null) {
            return null;
        }

        if (node.left == null) {
            return node.right;
        }

        if (node.right == null) {
            return node.left;
        }

        var tempNode = getMin(node.right);
        node.data = tempNode.data;
        node.right = removeNode(node.right,tempNode.data);
        return node;
    }
}

{% endhighlight %}

---

### 第十一章 图

图的定义：图由边的集合及顶点的集合组成。边由顶点对（v1,v2）定义，v1和v2分别是图上的顶点，顶点也有权重，也称为成本，如果一个图的顶点对是有序的，则可以称之为有向图。

图的实际信息都表示在边上，图的结构相比树来说灵活很多，一个顶点既可以由一条边，也可以有多条边与它相连。

图的表现形式有邻接表和邻接矩阵。

比较难，暂时放一放。

### 第十二章 排序算法

__基本排序算法__

- 冒泡排序
- 选择排序
- 插入排序

{% highlight javascript %}
Array.prototype.bubble = function() {
    var len = this.length;
    for (var i=len-1; i>=1; i--) {
        for (var j=0; j<i; j++) {
            if (this[j]>this[j+1]) {
                var t = this[j];
                this[j]=this[j+1];
                this[j+1]=t;
            }
        }
    }
    return this;
}

// 区别在于交换次数，每次交换和只交换一次。
Array.prototype.selection = function() {
    var len = this.length;
    var min;
    for (var i=0; i<len-1; i++) {
        var min = i;
        for (var j=i+1; j<len; j++) {
            if(this[j]<this[min]) {
                min = j;
            }
        }

        var t = this[i];
        this[i] = this[min];
        this[min] = t;
    }
    return this;
}



Array.prototype.insertion = function() {
    var len = this.length;
    var t;
    for(var i=1; i<len; i++) {
        for(var j=i; j>0; j--) {
            if(this[j]<this[j-1]) {
                t = this[j-1];
                this[j-1] = this[j];
                this[j] = t;
            }
        }
    }
    return this;
}
{% endhighlight %}

__基本排序算法的时间复杂度__

选择排序和插入排序要比冒泡排序快，插入排序是这三种中最快的。这主要取决于两个方面：

1. 比较开销选择排序的比较开销是固定的n(n-1)/2,而插入排序平均下来是n(n-1)/4.
2. 交换开销选择排序最多只需要执行2*(n-1)次交换，而插入排序平均的交换开销也是n(n-1)/4.这就取决于单次比较和交换的开销之比。如果是一个量级的，则插入排序优于选择排序，如果交换开销远大于插入开销，则插入排序可能比选择排序慢。

__高级排序算法__

希尔排序：其核心理念与插入排序不同，它会首先比较距离较远的元素，而非相邻的元素，和简单的比较相邻元素相比，使用这种方案可以使距离正确位置很远的元素更快的回到合适的位置。

工作原理：通过定义一个间隔序列来表示排序过程中进行比较元素之间有多远的间隔，我们可以动态的定义间隔序列。

{% highlight javascript %}
Array.prototype.shellsort() = function () {
    var len = this.length;
    var h = 1;
    while (h < len/3) {  // 动态计算间隔
        h = 3*h + 1;
    }

    while (h >= 1) {
        for (var i=h; i<len; i++) {
            for (var j=i; j>=h&&this[j]<this[j-h]; j-=h) {
                var t = this[j-h];
                this[j-h] = this[j];
                this[j] = t;
            }
        }
        h = (h-1)/3;
    }
}
{% endhighlight %}

归并排序：先递<span class='red'>归</span>分解数列，再合<span class='red'>并</span>数列就完成了<span class='red'>归并</span>排序。

归并排序的实现主要有两种策略:

- 自顶向下的归并排序，使用递归来实现，对于js来说这种方式不太可行，这个算法递归深度对它来说太深了。
- 自底向上的归并排序，首先将数据集分解为一组只有一个元素的数组，然后通过创建一组左右数组将它们慢慢合并起来。

{% highlight javascript %}
// 合并两个已经排序的数组，动态指针
function merge(left,right) {
    var leftLen = left.length;
    var rightLen = right.length;
    var i = j = 0;
    var result = [];

    while(i<leftLen&&j<rightLen) {
        if(left[i] < right[j]) {
            result.push(left[i]);
            i++;
        } else {
            result.push(right[j]);
            j++;
        }
    }

    while (i<leftLen) {
        result.push(left[i]);
        i++;
    }

    while (i<rightLen) {
        result.push(right[j]);
        j++;
    }
}
{% endhighlight %}

Javascript实现的自底向上归并排序算法

{% highlight javascript %}
function mergeSort(arr) {
    if(arr.length < 2) {
        return arr;
    }

    var step = 1;
    var left,right;
    while (step < arr.length) {
        left = 0;
        right = step;
        while(right+step<=arr.length) {
            mergeArrays(arr,left,left+step,right,right+step);
            left = right + step;
            right = left +step;
        }

        if(right < arr.length) {
            mergeArrays(arr,left,left+step,right,arr.length);
        }

        step *= 2;
    }
}

function mergeArrays(arr,startLeft,stopLeft,startRight,stopRight) {
    var rightArr = new Array(stopRight - startRight + 1);
    var leftArr = new Array(stopLeft - startRight + 1);
    k = startRight;
    for (var i=0; i<(rightArr.length-1); i++) {
        rightArr[i] = arr[k];
        k++;
    }

    k = startLeft;
    for (var i=0; i<(leftArr.length-1); i++) {
        leftArr[i] = arr[k];
        k++
    }

    rightArr[rightArr.length-1] = Infinity;
    leftArr[leftArr.length-1] = Infinity;

    var m=0;
    var n=0;
    for (var k=startLeft; k<stopRight; k++) {
        if (leftArr[m]<=rightArr[n]) {
            arr[k] = leftArr[m];
            m++;
        } else {
            arr[k] = rightArr[n];
            n++;
        }
    }
}

//////////////////////////////////////递归形式实现
Array.prototype.merge_sort = function() {
    // 这段merge的代码写的不错
    var merge = function(left,right) {
        var final = [];
        while(left.length&&right.length) {
            final.push(left[0]<=right[0]?left.shift():right.shift());
        }
        return final.concat(left.concate(right));
    }

    var len = this.length;
    if(len<2) return this;  // 递归必备，终止条件
    var mid = len / 2;
    return merge(this.slice(0,parseInt(mid)).merge_sort(),this.slice(parseInt(mid)).merge_sort());
}
{% endhighlight %}

---

### 第十四章 高级算法

__动态规划__

{% highlight javascript %}
// 使用动态规划实现斐波那契数列

// 递归形式
function fib(n) {
    if(n<2) {
        return n;
    }

    var pre = 0;
    var cur = 1;
    var result = 0;

    for(int i=0; i<n; i++) {
        result = pre + cur;
        pre = cur;
        cur = result;
    }

    return result;
}

function fib(n) {
    var val = [];
    for(var i=0; i<=n; i++) {
        val[i] = 0;
    }

    if(n==1||n==2) {
        return 1;
    } else {
        val[1] = 1;
        val[2] = 2;
        for(var i=3; i<=n; i++) {
            val[i] = val[i-1] + val[i-2];
        }

        return val[n-1];
    }
}

// 动态规划实现最长公共子串函数 -->二维数组保存结果

function lcs(word1,word2) {
    var max = 0;
    var index = 0;
    var lcsarr = new Array(word1.length + 1);
    for(var i=0; i <= word1.length+1; i++) {
        lcsarr[i] = new Array(word2.length+1);

        for(var j=0; j<=word2.length+1; j++) {
            lcsarr[i][j] = 0;
        }
    }

    for(var i=0; i<=word1.length+1; i++) {
        for(var j=0; j<=word2.length+1; j++) {
            if(i==0||j==0) {
            lcsarr[i][j] = 0;
        } else {
            if(word1[i-1]==word2[j-1]) {
                lcsarr[i][j] = lcsarr[i-1][j-1] + 1;
            } else {
                lcsarr[i][j] = 0;
            }
        }
        }
        if(max < lcsarr[i][j]) {
            max = lcsarr[i][j];
            index = i;
        }
    }

    var str = '';
    if(max==0) {
        return '';
    } else {
        for (var i=index - max; i<=max; i++) {
            str + word2[i];
        }

    return str;
    }
}

{% endhighlight %}


