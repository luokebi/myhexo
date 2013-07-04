title: 关于null &gt;= 0
date: 2013-07-04 16:49:35
tags: [javascript]
---
前两天看大神们在讨论一个问题,这个问题是这样的:
```js
null > 0// false 
null == 0// false 
null >= 0//true
```
为什么会这样呢? <br/>
<!-- more -->
###总结如下：
他们查阅了ES3和ES5对于关系运算符 ">" , "<", ">=", "<=" 以及相等运算符的实现：

1. ">=" 和 ">" 属于关系运算符
2. "==" 属于相等运算符
1. 关系运算符和相等运算符不是一类，实现算法不相同
2. 关系运算符总是将运算元尝试转化成number,然后再进行比较，而相等运算符则没有类型转化这个过程

###再看上面那个问题:
```js
null > 0 // null转化为number类型，相当于+null的值，为0，所以为false
null >= 0 // 通上，0 >= 0,为true
null == 0 // 没有类型转化，所以为false
```
这样就解释了为什么是这个结果。


大神更详细的分析以及与javascript作者的讨论:  <br/>
[http://www.cnblogs.com/_franky/archive/2012/09/26/2703723.html](http://www.cnblogs.com/_franky/archive/2012/09/26/2703723.html)
