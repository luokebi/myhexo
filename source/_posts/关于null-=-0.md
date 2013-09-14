title: 关于null &gt;= 0
date: 2013-07-04 16:49:35
tags: [javascript]
---
前两天看大神们在讨论一个问题,这个问题是这样的:
```js
null > 0// false
null >= 0//true 
null == 0// false 
```
为什么会这样呢? <br/>

照理说
```js
null > 0 //false
null >= 0 // true
```
应该可以推出：
```js
null == 0 // true (实际上并不是如此)
```
<!-- more -->
###总结如下：
他们查阅了ES3和ES5对于关系运算符 ">" , "<", ">=", "<=" 以及相等运算符的实现：

1. ">=" 和 ">" 属于关系运算符
2. "==" 属于相等运算符
1. 关系运算符和相等运算符不是一类，实现算法不相同
2. 关系运算符总是将运算元尝试转化成number,然后再进行比较，而相等运算符则没有类型转化这个过程（但不代表“0” == 0 // true 有错）
1. 最关键的一点，不要把 拿 a > b ,  a == b 的结果 想当然的去和 a >= b 建立联系. 正确的符合最初设计思想的关系是  a > b 与 a >= b是一组 . a == b 和其他相等运算符才是一组。 比如  a === b , a != b, a !== b 。

###再看上面那个问题:
```js
null > 0 // null转化为number类型，相当于+null的值，为0，所以为false
null >= 0 // 通上，0 >= 0,为true
null == 0 // 由于null在设计上在此不尝试类型转化，所以为false
```
这样就解释了为什么是这个结果。


大神更详细的分析以及与javascript作者的讨论以及js在比较时的规则请看:  <br/>
[http://www.cnblogs.com/_franky/archive/2012/09/26/2703723.html](http://www.cnblogs.com/_franky/archive/2012/09/26/2703723.html)
