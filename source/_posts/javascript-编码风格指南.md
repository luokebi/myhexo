title: JavaScript 编码风格指南
date: 2013-07-01 20:48:51
tags: [javascript, 编码风格]
---
![](/img/maintainableJavascript.jpg)

&emsp;这几天一直在读这本[《编写可维护的JavaScript》](http://book.douban.com/subject/21792530/)
,觉得编码风格对于一个长期维护的软件而言是非常的重要的。下面我就列举一下书中建议的一些规范(下面的写法均为推荐写法)。

##缩进
每一行由四个空格组成,避免使用Tab进行缩进。
```javascript
if (true) {
    doSomething();
}
```
##行的长度
每行不应超过80个字符。如果一行多于80个字符，应当在一个运算符后换行。下一行增加两级缩进（8个字符）
```js
doSomething(argument1, argument2, argument3, argument4,
        argument5);
```
<!-- more -->
##原始值
字符串应当始终使用双引号且保持一行
```js
var name = "Nicholas";
```
数字应当使用十进制证书，科学技术法表示整数，十六进制证书，或者十进制浮点小数，小数点前后应当至少保留以为数字。避免使用八进制直接量。
```js
var count = 10;

var price = 10.0;
var price = 10.00;

var num = 0xA2;

var num = 1e23;
```
##运算符间距
二元运算符前后必须使用一个空格来保持表达式的整洁。操作符包括赋值运算符和逻辑运算符。
```js
var found = (values[i] === item);

if (found && (count > 10)) {
    doSomething();
} 

for (i = 0; 1 < count; i++) {
    process(i);
}
```
##括号间距
当使用括号时，紧接左括号之后和紧接右括号之间不应该有空格。
```js
var found = (values[i] === item);

if (found && (count > 10)) {
    doSomething();
} 

for (i = 0; 1 < count; i++) {
    process(i);
}
```
##对象直接量
* 起始左花括号应当与表达式同一行
* 每个属性的名值应该保持一个缩进，每一个属性应当在左花括号后另起一行
* 不含引号的属性名
* 倘若属性值是函数类型，函数体应当在属性名之下另起一行，而且其前后均保留一个空行
* 一组相关属性前后可插入空行以提升代码的可读性
* 结束右花括号应该独占一行
```js
var object = {
    key1: value1,
    key2: value2,

    func: function() {
        // do something
    },

    key3: value3
};
```

但贵姓字面量作为函数参数时：
```js
doSomething({
    key1: value1,
    key2: value2
});
```
##注释
###单行注释
* 单行注释前有一空行
* “//”与文字之间有一个空格
```js
if (condition) {
    
    // comments here
    allowed();
}
```
###多行注释
* 首行以/*开始，不包含其他文字
* 接下来的行以*开头，这些行可以有文字
* 最后一行以*/开头，同样不包含其他文字
* 每个多行注释前也有一个空行
```js
if (condition) {
    
    /*
    * comment here
    * comment here
    */
    allowed();
}
```
##注释声明
```js
// TODO: 我希望找到一种更快的方式
doSomething();

/*
* HACK: something ..,
* something....
*/
if (document.all) {
    doSomething();
}

// REVIEW: 有更好的方法吗？
if (document.all) {
    doSomething();
}
```
##变量声明
所有的变量声明都应该在使用前先定义。变量定义应该放在函数的开头。使用单var模式的表达式。未初始化的变量应放在最后。
```js
var count = 10,
    name = "Nicholas",
    fount = false,
    empty;
```
##函数声明
函数也应当使用之前定义。一个不是作为方法的函数应当使用函数定义的格式。函数名和开始圆括号之间不应当有空格。结束圆括号和右边的花括号之间应该有一个空格。参数名之间应该在都好之后保留一个空格。
```js
function doSomething(arg1, arg2) {
    return arg1 + arg2;
}
```
匿名函数可能作为方法赋值给对象，或者作为其他函数的参数。funtion关键字与开始圆括号之间不应该有空格。
```js
object.method = function() {

    // code here
}
```
立即执行函数应该在行数调用的外层用圆括号包裹。
```js
var value = (function(){
	
    // 函数体
    return {
        message: "Hi"
    }
}());
```
##命名
推荐采用驼峰式命名
```js
var accountNumber = "8401-1";

function doSomething() {
	
	// code here
}
```
构造函数
```js
function myObject() {
	
	// code here
}
```
常量命名应当是所有字母大写，不同单词之间使用下划线隔开。
```js
var TOTAL_COUNT = 10;
```
对象的属性同变量命名规则相同，方法与函数规则相同。如果属性或者方法是私有的，应该在其前面加一个下划线。
```js
var object = {
    _count: 10,

    _getCount: function() {
        return this._count;
    }
}
```
##赋值
当变量赋值时，如果右边为表达式，需要用圆括号包裹起来。
```js
var flag = (i < count);
```
##三元操作符
三元操作符应当仅仅用在条件赋值语句中，而不要作为if的代替品。
```js
var value = condition ? value1 : value2;
```
##语句
###if语句
决不允许省略花括号
```js
if (condition) {
    statements
}

if (condition) {
	statements
} else {
    statements
}

if (condition) {
    statements
} else if (condititon) {
    statements
} else {
    statements
}
```
###for语句
for语句的初始化不应该有变量声明
```js
var i,
    len;

for (i = 0, len = 10; i < len; i++) {
	
	// code here
}

```
###while 语句
```js
while (condition) {
    statements
}
```

###do while语句
```js
do {
    statements
} while (condition)
```
###switch语句
```js
switch (expression) {
    case expression:
        statements

    case expression:
        statements

    default:
        statements
}
```
如果没有default,应该使用一行注释代替。
```js
switch (expression) {
    case expression:
        statements

    case expression:
        statements

    // no default
}
```
###try语句
```js
try {
    statements
} catch (variable) {
    statements
} finally {
    statements
}
```