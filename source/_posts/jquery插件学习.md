title: jQuery插件学习
date: 2013-07-29 22:00:37
tags:
---

## 一般jQuery插件的写法:
一个jQuery插件实际上就是对jQuery命名空间的扩展，我们在扩展原生JavaScript方法的时候通过在原生对象的prototype上扩展方法，比如在Array对象上扩展一个数组去重的方法，我们使用：
```js
Array.prototype.removeDuplicates = function () {
    var temp = {},
        r = [],
        i,k;
        
    for (i = 0;i < this.length;i++) {
      temp[this[i]] = true;
    }
    var r = [];
    for (k in temp) {
        r.push(k);
    }
    return r;
}
```
这样我们任何一个数组都可以调用这个方法来去重：
```js
var arr = ["apple","pair","grape","pair","apple"]
console.log(arr.removeDuplicates()); // ["apple","pair","grape"]
```
而`jQuery.fn`是`jQuery.prototype`的简写，所以我们用`jQuery.fn.pluginName = function`来创造一个jQuery插件：
```js
// 基本jQuery插件形式1
function($) {
 
    // 向jQuery中被保护的“fn”命名空间中添加你的插件代码，用“pluginName”作为插件的函数名称
    $.fn.pluginName = function(options) {
 
        // 返回“this”（函数each（）的返回值也是this），以便进行链式调用。
        return this.each(function() {
 
            // 此处运行代码，可以通过“this”来获得每个单独的元素
            // 例如： $(this).show()；
            var $this = $(this);
 
        });
 
    }
 
})(jQuery);
```

除了这种，可以借助extend:
```js
// 基本jQuery插件形式2
function($) {
 
    $.fn.extend({
 
        pluginNmae:function(options) {
 
          return this.each(function() {
 
              var $this = $(this);
 
          });
 
        }
    });
 
})(jQuery);
```
这样我们就可以使用jQuery插件了：
```js
$('selector').pluginName();
```
## 传入配置：



## 有公有方法和私有方法的写法:
有些时候我们需要一些函数被插件外的代码调用；一些函数却不能让外部的代码可以访问。这也就是我们说的公有方法和私有方法。
一般我们在插件中定义的函数外部都是访问不了的，所以我们要将一些暴露出去。实现这个这里有两种方法：

方法一：我们用一个对象来存储公有函数：
```js
// code here
``` 
方法二：在javascript中所有都是对象，函数也是一个对象，所以可以直接把公有函数直接写在`$.fn.pluginName`上：
```js
// code here
```