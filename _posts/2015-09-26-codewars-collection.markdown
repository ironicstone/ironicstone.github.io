---
layout: post 
  
title: Codewars集合贴
date:  2015-10-11 19:07:27
summary:
categories: codewars
tags: js
---

> 收集一些巧妙的回答，经常回顾一下，有点意思

## 第13题

*Description*

>In this kata we want to convert a string into an integer. The strings simply represent the numbers in words.

>Examples:

>"one" => 1
>"twenty" => 20
>"two hundred forty-six" => 246
>"seven hundred eighty-three thousand nine hundred and nineteen" => 783919

*Best Practices*

{% highlight javascript %}
var words = {
  "zero":0, "one":1, "two":2, "three":3, "four":4, "five":5, "six":6, "seven":7, "eight":8, "nine":9,
  "ten":10, "eleven":11, "twelve":12, "thirteen":13, "fourteen":14, "fifteen":15, "sixteen":16,
  "seventeen":17, "eighteen":18, "nineteen":19, "twenty":20, "thirty":30, "forty":40, "fifty":50,
  "sixty":60, "seventy":70, "eighty":80, "ninety":90
};
var mult = { "hundred":100, "thousand":1000, "million": 1000000 };
function parseInt(str) {
  return str.split(/ |-/).reduce(function(value, word) {
    if (words[word]) value += words[word];
    if (mult[word]) value += mult[word] * (value % mult[word]) - (value % mult[word]);
    return value;
  },0);
}
{% endhighlight %}

## 第12题

*Description*

>Example: pickPeaks([3,2,3,6,4,1,2,3,2,1,2,3]) returns {pos:[3,7],peaks:[6,3]}

*My Solution*

{% highlight javascript %}
function pickPeaks(arr){

  //  return {pos:[],peaks:[]}
  // 我做的感觉好复杂。。。
  var result = {
  pos:[],
  peaks:[]
  };
  var i,j,k;

 for(i=1,j=0; i<arr.length-1; i++)
 {
   if(arr[i]>arr[i-1])
   {
    if(arr[i]>arr[i+1])
    {
     result.pos.push(i);
     result.peaks.push(arr[i]);
     }
     else if(arr[i]<arr[i+1]) continue;
     else
     {
        j = i;
        while(arr[j]==arr[i]) j++;
        if(arr[j] < arr[i])
        {
         result.pos.push(i);
         result.peaks.push(arr[i]);
        }
     }
   }
 }
 return result;
}
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
function pickPeaks(arr){
  var result = {pos: [], peaks: []};
  if(arr.length > 2) {
    var pos = -1;
    for(var i=1; i<arr.length;i++){
      if(arr[i] > arr[i-1]) {
        pos = i;
      } else if(arr[i] < arr[i-1] && pos != -1) {
        result.pos.push(pos);
        result.peaks.push(arr[pos]);
        pos = -1;
      }
    }
  }
  return result;
}

function pickPeaks(arr){

  var pos = [], peaks = [], lastIncreaseIndex = null;
  arr.forEach(function(e, index) {

    if(lastIncreaseIndex) {

      if(arr[index - 1] > arr[index]) {
        pos.push(lastIncreaseIndex);
        peaks.push(arr[lastIncreaseIndex]);
        lastIncreaseIndex = null;
      }
    }
    if(index && arr[index] > arr[index - 1]) {
      lastIncreaseIndex = index;
    }
  });
  return {pos:pos,peaks:peaks};
}

function pickPeaks(arr){
  var result = arr.reduce(function (acc, n, i) {
    if (n > acc[2]) return [acc[0], acc[1], n, i, true];
    if (n === acc[2]) return [acc[0], acc[1], n, acc[3], acc[4]];
    else return acc[4]
      ? [acc[0].concat([acc[3]]), acc[1].concat([acc[2]]), n, i, false]
      : [acc[0], acc[1], n, i, false];
  }, [[], [], Number.MAX_VALUE, -1, false]);
  return { pos: result[0], peaks: result[1] };
}
{% endhighlight %}

- <span class="red">11</span>

*Description*

> You are in a room with 100 vending machines, each one numbered 0 to 99. Each vending machine has 100 candy bars inside. For 99 of the vending machines, all of their candy bars weigh 100 grams, but one special machine has candy bars that weigh 101 grams.

> Candy bars you vend are put into a pile, but you can only weigh the pile at most 10 times.

> Determine the number of the machine with the heavy candy bars.


*Best Practices*

{% highlight javascript %}
//一次就可以分辨出那个有问题
function findSpecialIdx(vms) {
  // Solution requiring only a single weighing
  for (var i = 0; i < 100 ; i++) {
    for (var j = 0; j < i; j++) {
      vms[i].vend();
    }
  }
  return vms.weigh() % 100;
};

///////////////////////////////////////////////////////////////////
function findSpecialIdx(vms) {
  var expectedWeight = 0;
  var low = 0;
  var high = 100;
  while (low+1 < high) {
    var mid = Math.floor( (low + high) / 2 );
    for (var i=0; i < mid; ++i) {
      vms[i].vend();
      expectedWeight += 100;
    }
    var weight = vms.weigh();
    if (weight == expectedWeight) {
      low = mid;
    } else {
      high = mid;
      expectedWeight = weight;
    }
  }
  return low;
};

//////////////////////////////////////////////////////////
function findSpecialIdx(vms)
{
  var previousWeight, currentWeight = 0;
  var startIdx = 0;
  var endIdx = 99;
  while (startIdx != endIdx)
  {
    middelIdx = getMiddelIdx(startIdx, endIdx);
    makePile(vms, startIdx, middelIdx);
    previousWeight = currentWeight;
    currentWeight = vms.weigh();
    (currentWeight - previousWeight) % 100 === 0 ? startIdx = middelIdx + 1 : endIdx = middelIdx;  // %100 的原因是有可能多一罐，这里要注意
  }
  return startIdx;
}

function getMiddelIdx(startIdx, endIdx)
{
  return startIdx + Math.floor((endIdx - startIdx) / 2);
}

function makePile(vms, startIdx, middelIdx)
{
  for (var idx = startIdx; idx <= middelIdx; idx++)
    vms[idx].vend();
}
{% endhighlight %}

- <span class="red">10</span>

*Description*

>Number is a palindrome if it is equal to the number with digits in reversed order. For example, 5, 44, 171, 4884 are palindromes and 43, 194, 4773 are not palindromes.
Write a method palindrome_chain_length which takes a positive number and returns the number of special steps needed to obtain a palindrome. The special step is: "reverse the digits, and add to the original number". If the resulting number is not a palindrome, repeat the procedure with the sum until the resulting number is a palindrome.

*Best Practices*

{% highlight javascript %}
function palindromeChainLength(n) {
  var count = 0, rev = 0;
  while(n != (rev = parseInt(n.toString().split('').reverse().join('')))) {
    n += rev;
    count++;
  }
  return count;
};

///////////////////////////////////////////////////
/*使用闭包*/
var palindromeChainLength = function (n) {
    var chain = 0;
    var find = function (num) {
        var s = num + '';
        var s_rev = s.split('').reverse().join('');
        if (s_rev == s)
            return chain;
        chain++;
        return find(+s_rev + num);  // tips:+'12' ---> 12,字符串快速变数字，而不用parseInt,挺好
    };
    return find(n);
};
{% endhighlight %}

<!-- 往期回顾 -->

- <span class="red">9</span>

*Description*

> 缓存函数

*My Solution*

{% highlight javascript %}
function cache(func) {
  // do your magic here
   var wrapper = {};
   return function() {
    var params = JSON.stringify(arguments);
         if (!(params in wrapper))
      {wrapper[params] = func.apply(null,arguments);}
      return wrapper[params];
}
}
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
同样是闭包的使用，与我的类似，不做引用
{% endhighlight %}

- <span class="red">8</span>

*Description*

>计算水仙花数

*My Solution*

{% highlight javascript %}
function narcissistic( value ) {
  // Code me
  var digits = value.toString().split("").map(Number);
  var power = digits.length;
  return  digits.reduce(function(s,c){
    return s + Math.pow(c,power);
  },0) === value ? true:false;
}
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
function narcissistic( value ) {
  return ('' + value).split('').reduce(function(p, c){
    return p + Math.pow(c, ('' + value).length)
    }, 0) == value;
}         // 没有多余的参数
{% endhighlight %}

- <span class="red">7</span>

*Description*

>If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.
Finish the solution so that it returns the sum of all the multiples of 3 or 5 below the number passed in.

*My Solution*

{% highlight javascript %}
// 哈哈，这题用了容斥原理来做。
function solution(number){
    number = number - 1;
  var threes = Math.floor(number/3);
  var fives = Math.floor(number/5);
  var both = Math.floor(number/15);
  return getSum(threes)*3 + getSum(fives)*5 - getSum(both)*15;

  function getSum(value){
    return (value+1)*value/2;
  }
}
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
//与我的方法类似，不做引用了
{% endhighlight %}

- <span class="red">6</span>

*Description*

> Given a positive integer of up to 16 digits, return true if it is a valid credit card number, and false if it is not.

*My Solution*

{% highlight javascript %}
function validate(n){
  var digits = n.toString().split("");
   var sum =  digits.reverse().map(function(v,i){
        if(i%2) {
            var temp = 2 * parseInt(v);
            if (temp > 9){
                // return temp.toString().split("").reduce(function(c,n){return parseInt(c) + parseInt(n);});
                  return temp - 9; //发生进位，数字和减9
            }
            else
                return temp;  //经常忘了else的处理
        }
        else {
            return parseInt(v); //经常忘了else的处理，这里出现两次这个错误，不应该。
        }
    }).reduce(function(c,n){return c + n;})%10;
 console.log(sum);
 return !sum;
}
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
function validate(n){
  var sum = 0;

  while (n > 0) {
    var a = n % 10;
    n = Math.floor(n / 10);

    var b = (n % 10) * 2;
    n = Math.floor(n / 10);

    if (b > 9) {
      b -= 9;
    }

    sum += a + b;
  }

  return sum % 10 == 0;
}

///////////////////////////////////////////////
function validate(n) {
  n = n.toString().split('').map(Number).reverse(); //这里的map用的太好了，赞。
  return n.reduce(function (sum, digit, index) {
    if (index & 1) digit <<= 1;   // 通过与运算判断奇偶，位运算符来做乘法
    if (digit > 9) digit -= 9;    // 效果与下面的类似
    /////////////////////////////////
    // if (index % 2) digit *= 2; //
    // if (digit > 9) digit -= 9;  //
    /////////////////////////////////
    return sum + digit;
  }, 0) % 10 == 0;
}
{% endhighlight %}

- <span class="red">5</span>

*Description*

{% highlight javascript %}
titleCase('a clash of KINGS', 'a an the of') // should return: 'A Clash of Kings'
titleCase('THE WIND IN THE WILLOWS', 'The In') // should return: 'The Wind in the Willows'
titleCase('the quick brown fox') // should return: 'The Quick Brown Fox'
{% endhighlight %}

*My Solution*

{% highlight javascript %}
function titleCase(title, minorWords) {
  var aTitle = title.toLowerCase().split(" ");
  if(minorWords)
  { var aMinor = minorWords.toLowerCase().split(" ") }

  for( var j = 0;j<aTitle.length;j++){
      if(minorWords)
  {if(aMinor.indexOf(aTitle[j]) == -1){
          var temp = toUpper(aTitle[j]);
            console.log(temp);
          aTitle[j] = temp;
      }  }
     else {
 var temp = toUpper(aTitle[j]);
            console.log(""+j+ temp);
          aTitle[j] = temp;
}


}
   aTitle[0] = toUpper(aTitle[0]);
   return aTitle.join(" ");

  function toUpper( word ) {
    var arr = word.split("");
    console.log("word: " + word);
    for ( var i = 0; i < arr.length; i++){
      if( i == 0)
           { arr[0]=arr[0].toUpperCase();}
      }
console.log(arr.join(""));
      return arr.join("");
    }


}
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
function titleCase(title, minorWords) {
  var minorWords = typeof minorWords !== "undefined" ? minorWords.toLowerCase().split(' ') : [];
  return title.toLowerCase().split(' ').map(function(v, i) {
    if(v != "" && ( (minorWords.indexOf(v) === -1) || i == 0)) {
      v = v.split('');
      v[0] = v[0].toUpperCase();
      v = v.join('');
    }
    return v;
  }).join(' ');
}

/////////////////////////////////////////////////////////

String.prototype.getWords = function () {
  return this.match(/\b\w+/gi) || [];
};
String.prototype.ucFirst = function () {
  return this.charAt(0).toUpperCase() + this.slice(1)
}
Array.prototype.toLowerCase = function () {
  return this.map(function (word) {
    return word.toLowerCase();
  });
};

function titleCase(title, minorWords) {
  if (typeof title !== 'string') {
    return '';
  }
  if (typeof minorWords !== 'string') {
    minorWords = '';
  }
  var
    words = title.getWords().toLowerCase(),
    minorWords = minorWords.getWords().toLowerCase();

  return words.map(function (word, index) {
    return (minorWords.indexOf(word) >= 0 && index > 0) ? word : word.ucFirst();
  }).join(' ');
}

/////////////////////////////////////////////////////////

String.prototype.capitalize = function() {
    return this.charAt(0).toUpperCase() + this.slice(1).toLowerCase();
}

function titleCase(title, minorWords) {
  var titleAr = title.toLowerCase().split(' '),
      minorWordsAr = minorWords ? minorWords.toLowerCase().split(' ') : [];

  return titleAr.map(function(e, i){return minorWordsAr.indexOf(e) === -1 || i === 0 ? e.capitalize() : e })
                .join(' ');
}

/////////////////////////////////////////////////////////

function capitalize(w) {
  return w.charAt(0).toUpperCase() + w.slice(1);
}

function titleCase(title, minorWords) {
  if (title.indexOf(' ') === -1) {
    return capitalize(title);
  }

  var titleArr = title.split(' ');

  var minorWordsArr = typeof minorWords === 'undefined' ? [] : minorWords.split(' ').map(function(w) {
    return w.toLowerCase();
  });

  return capitalize(titleArr.map(function(w) {
    w = w.toLowerCase();
    if (minorWordsArr.indexOf(w) > -1) {
      return w;
    }

    return capitalize(w);
  }).join(' '));
}
{% endhighlight %}

- <span class="red">4</span>

*Description*

>Finish the solution so that it takes an input 'n' (integer) and returns a string that is the decimal representation of the number grouped by commas after every 3 digits.

{% highlight javascript %}

       1  ->           "1"
      10  ->          "10"
     100  ->         "100"
    1000  ->       "1,000"
   10000  ->      "10,000"
  100000  ->     "100,000"
 1000000  ->   "1,000,000"
35235235  ->  "35,235,235"
{% endhighlight %}

*My Solution*

{% highlight javascript %}
function groupByCommas(n) {
    var arr = (n + "").split("");
    if( arr.length < 4){
        return n+"";
    }
    else{
       var temp = arr.reverse().join("").match(/[0-9]{3}?/g);
       var tail = (n + "").split("").reverse().join("").substr(temp.length * 3).split("").reverse().join("");
       console.log(temp + tail);
       return tail + "," + temp.join(",").split("").reverse().join("");
    }
}

{% endhighlight %}

*Best Practices*

{% highlight javascript %}
function groupByCommas(n) {
  return n.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
}

/////////////////////////////////////////////////////////

function groupByCommas(n) {
  var s = n.toString(),
      r = [];

  // reverse number string so we can easily count up in blocks of 3
  s = reverse(s);

  for (var i = 0, l = s.length; i < l; i += 3) {
    r.push(s.substr(i, 3));
  }

  // combine the groups of 3 numbers into string, then reverse back to original order
  return reverse(r.join(','));
}

function reverse(s) {
  return s.split('').reverse().join('');
}

/////////////////////////////////////////////////////////

function groupByCommas(n) {
  str = (""+n).split("");
  for(var i = str.length - 3; i > 0 ; i-=3){
     str.splice(i,0,',');
  }
  return str.join('')
}
{% endhighlight %}

- <span class="red">3</span>

*Descirption*

> Your task is to sort a given string. Each word in the String will contain a single number. This number is the position the word should have in the result.

> Note: Numbers can be from 1 to 9. So 1 will be the first word (not 0).

> If the input String is empty, return an empty String. The words in the input String will only contain valid consecutive numbers.

> For an input: "is2 Thi1s T4est 3a" the function should return "Thi1s is2 3a T4est"

*My Solution*

{% highlight javascript %}
function order(words){
  // ...
  var arr = words.split(" ");
  return arr.sort(function(a,b){
      var num1 = a.match(/[0-9]/g);
      var num2 = b.match(/[0-9]/g);
      return num1 - num2;
      }).join(" ");
}
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
function order(words){
  return words.split(' ').sort(function(a, b){
      return a.match(/\d/) - b.match(/\d/);
   }).join(' ');
}
{% endhighlight %}

- <span class="red">2</span>

*Description*

> Order People by age Using Arrow Function

*My Solution*

{% highlight javascript %}
var OrderPeople = function(people){
  return people.sort( (a,b) => {return a.age - b.age;} ); //complete this function
}
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
var OrderPeople = function(people){
  return people.sort((a,b) => a.age - b.age );
}
{% endhighlight %}

- <span class="red">1</span>

*Description*

>Jim, the rocket scientist has finished the code for the board computer of his new Mars rocket. Only one last function is missing, the function for creating the countdown. Because Jim is always nervous when starting a new rocket, he sometimes forgets a number when doing the final countdown from ten to zero, so in order to be sure everything will work perfectly on the great day, he wrote a JavaScript function that does the countdown for him.

> Today is rocket launch day, and Jim has a big problem. The unit test for the countdown suddenly fails! Jim could swear that the unit tests were all green yesterday, and that he didn't change anything in the countdown function. He suspects that his assistant Jeff has introduced a bug somewhere in the rocket board computer startup code, but he cannot understand how that could affect the output of his countdown method in such a strange way. Oh, if Jim and Jeff had only learned a bit more JavaScript working on Codewars katas!

> Jim is desperate because the countdown function has already been burned into a ROM deep inside the board computer that cannot be easily replaced. He has to fix the problem by calling a method in the startup code that is still accessible. Can you help poor Jim?

> Here is Jim's countdown code that he is not able to change any more:

{% highlight javascript %}
function countdown() {
  var ret = "";
  var numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  for (var number in numbers) {
    if (ret) ret += ", ";
    if (number < 10)
      number = 10 - number;
    else if (number == 10)
      number = "Zero";
    ret += number;
  }
  ret += "!";
  return ret;
}
{% endhighlight %}
> Write a function fix_countdown() that contains a fix to make countdown() work again.

*My Solution*

{% highlight javascript %}
这个题不会做,这里面涉及到bug fix,函数没问题，出问题的只能是原型里，要学会调试和分析。
{% endhighlight %}

*Best Practices*

{% highlight javascript %}
function fix_countdown() {
  delete Array.prototype.Dammit;
}

// This code will run before the rocket starts
function fix_countdown() {
  // Please insert your bug fix here!
  if (typeof Array.prototype.Dammit !== 'undefined') delete Array.prototype.Dammit;
}

// Jim's final countdown function;
// may not be changed any more.
// (It used to work the day before!)
function countdown() {
  var ret = "";
  var numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  for (var number in numbers) {
    if (ret) ret += ", ";
    if (number < 10)
      number = 10 - number;
    else if (number == 10)
      number = "Zero";
    ret += number;
  }
  ret += "!";
  return ret;
}
{% endhighlight %}
[delete](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/delete)

[delete操作符](http://blog.charlee.li/javascript-variables-and-delete-operator/)
