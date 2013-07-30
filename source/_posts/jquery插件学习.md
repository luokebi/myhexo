title: jQuery插件学习
date: 2013-07-29 22:00:37
tags: [jQuery]
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
<!-- more -->
```js
// 基本jQuery插件形式1
function($) {
 
    // 向jQuery中被保护的“fn”命名空间中添加你的插件代码，用“pluginName”作为插件的函数名称
    $.fn.pluginName = function() {
 
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
 
        pluginNmae:function() {
 
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
很多时候我们需要自定义我们使用的插件，如颜色，位置等，来控制插件的行为。所以我们需要传入一个配置对象,当然我们也需要一个默认的配置：
```js
// 带配置jquery插件
function($) {

    var defualtOption = {
        propertyName1: 'value',
        propertyName2: "value2"
    };
 
    $.fn.pluginName = function(option) {

        return this.each(function() {
            var $this = $(this);
            var option = $.extend({},defaultOption,option || {});
            // code here
 
        });
 
    }
 
})(jQuery);
```
这样就是实现了传入配置。这里可以看到我们把`var defaultOption = {...}`写在了`$.fn.puginName = function(){}`的外面，这是为了让插件更高效，节约内存，把一次性的代码放在主循环之外。想了解更多关于jquery插件的优化可以看看这篇文章[10 Coding Tips to Write Superior jQuery Plugins](http://www.websanova.com/blog/jquery/10-coding-tips-to-write-superior-jquery-plugins#.Ufe-WY0ybEI)。



## 有公有方法和私有方法的写法:
有些时候我们需要一些函数被插件外的代码调用；一些函数却不能让外部的代码可以访问。这也就是我们说的公有方法和私有方法。
一般我们在插件中定义的函数外部都是访问不了的，所以我们要将一些暴露出去。实现这个这里有两种方法：

方法一：我们用一个对象来存储公有函数：
```js
// 包含公有方法和私有方法1
(function($) {
 
    var privateFunction = function() {
        // code here
    }
 
    // 通过字面量创造一个对象，存储我们需要的共有方法
    var methods = {
        // 在字面量对象中定义每个单独的方法
        init: function() {
 
            return this.each(function() {
            
                var $this = $(this);
 
                // 执行代码
                // 例如： privateFunction();
                // also: methods.publicFunction1();
            });
        },
       
        publicFunction1: function() {
         // code here
        },
        
        publicFunction2: function() {
          // code here
        }
    };
 
    $.fn.pluginName = function() {
        var method = arguments[0];
 
        // 检验方法是否存在
        if(methods[method]) {
 
            // 如果方法存在，存储起来以便使用
            // 注意：我这样做是为了等下更方便地t使用each（）
            method = methods[method];
 
        // 如果方法不存在，检验对象是否为一个对象（JSON对象）或者method方法没有被传入
        } else if( typeof(method) == 'object' || !method ) {
 
            // 如果我们传入的是一个对象参数，或者根本没有参数，init方法会被调用
            method = methods.init;
        } else {
 
            // 如果方法不存在或者参数没传入，则报出错误。需要调用的方法没有被正确调用
            $.error( 'Method ' +  method + ' does not exist on jQuery.pluginName' );
            return this;
        }
 
        // 调用我们选中的方法
        return method.call(this);
 
    }
 
})(jQuery);
``` 
这样我们就可以调用插件以及公有方法：
```js
$('selector').pluginName(); // 将会执行method.init()
$('selector').pluginName('publicFunctuon1'); // 将会执行公共方法1
$('selector').pluginName('publicFunctuon2'); // 将会执行公共方法2
```

方法二：在javascript中所有都是对象，函数也是一个对象，所以可以直接把公有函数直接写在`$.fn.pluginName`上：
```js
(function($){
  // 在我们插件容器内，创造一个公共变量来构建一个私有方法
    var privateFunction = function() {
        // code here
    }
    
    $.fn.pluginName = function() {
      
      return this.each(function(){
        var $this = $(this);
        // code here
        // for example: privateFunction();
        // also: $.fn.pluginName.publicFunction1();
      });
    
    }
    
    //public methods
    $.fn.pluginName.publicFunction1 = function() {
      // code here
    };
    
    $.fn.pluginName.publickFunction2 = function() {
      // code here
    };
 
}(jQuery);
```
在这里我们这样来调用公共方法：
```js
$('selector').pluginName.publicFunction1(); // 将会执行公共方法1
$('selector').pluginName.publicFunction2(); // 将会执行公共方法2
```