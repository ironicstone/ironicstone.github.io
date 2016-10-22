---
layout: post 
show:true
title:  字符编码问题
date:   2015-12-11 16:46:38
summary: python爬取数据时所遇到的字符编码问题
categories: python data
tags: python data
---
> 内容翻译自《Web Scraping With Python》

### 简要回顾一下编码类型

1990s,unicode编委会希望可以有一种编码格式可以表示所有的字符，包括中文（象形文字），日文，数学符号，甚至是一些特殊的标记。因此诞生了UTF-8(Universal Character Set - Transformation Format 8 bit)。这个名字有点误导，并不是所有的字符都是8位，而是最小是8位。UTF-8有一个标志器标志下一个字符是通过一个字符或是两个字符来表示。而且4个字符理论上可以表达所有的字符了。实际上现有共有1114112个字符，所以21bits就已经足够了，还没到3个字符。
UTF-8和Unicode之间有一种对应关系。具体可以参看[字符编码笔记](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)和[The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets](http://www.joelonsoftware.com/articles/Unicode.html).

然而ASCII码从1960s就可使用，使用了7bits，这个主要针对的是拉丁字符集，包括大写和小写，标点，也就是说你在你键盘上说有的字符都在这7bits中。这个和当时的背景有关---内存贵啊。

Unicode 与 UTF-8的转换关系<br>
0000 0000 - 0000 007F | 0xxxxxxx    ----->实际可用7bits<br>
0000 0080 - 0000 07FF | 110xxxxx 10xxxxxx    ----->实际可用11bit<br>
0000 0800 - 0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx    ----->实际可用16bits<br>
0001 0000 - 0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx    ----->实际可用21bits<br>

举两个例子：字母a的Unicode值为0X61,中文字符‘腾’的Unicode值为0X817E，转换步骤如下：

- a在第一行的范围，腾在第三行的范围
- 所以a的utf编码还是0x61,‘腾’的二进制位1000 0001 0111 1110，只需把这16位填入第三行中空出的16位中即可
- 所以‘腾’为1110 1000 1000 0101 1011 1110 ---> E 8 8 5 B E
- 结果：a->0x61  腾->0xE885BE

