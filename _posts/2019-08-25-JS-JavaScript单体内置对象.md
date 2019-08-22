---
layout:     post
title:      JS-JavaScript单体内置对象
subtitle:   JavaScript
date:       2019-08-25
author:     listen_spacer
header-img: img/background1.jpeg
catalog: true
tags:
    - JavaScript
---
## JavaScript单体内置对象

```
    ECMA-262对内置对象的定义是："由ECMAScript实现提供的、不依赖于宿主环境的对象，这些对象在ECMAScript程序执行之前就已经存在了。"（例如：Object、Array和String）
```
ECMA-262还定义两个单体内置对象：**Global** 和 **Math**

#### 1、Global对象

>不属于任何其他对象的属性和函数，最终都是他的属性和方法。
>所有在全局作用域中定义的属性和函数，都是Global对象的属性。
>诸如：isNaN()、isFinite()、parseInt()以及parseFloat()，实际上都是Global对象的方法。

**1.1、URI编码方法**

**encodeURI()** 和**encodeURIComponent()** 方法可以对URI进行编码，以便发送给浏览器。

```
有效的URI中不能包含某些字符，例如空格。		
但是通过这两个URI编码方法对URI进行编码，他们**用特殊的UTF-8 编码替换所有无效的字符**，从而让浏览器能够接受和理解。
```

>区别：
>encodeURI()不会对本身属于URI的特殊字符进行编码。
>encodeURIComponent()则会对它发现的任何非标准字符进行编码。

```
与这两个方法相对应的解码方法：
decodeURI()和decodeURIComponent()
```

注意：
编码后的URI才是有效的。

#### 2、eval()方法

这个方法只接收一个参数，即**要执行的ECMAScript(或JavaScript)字符串**。

>语句的执行结果会被插到eval()方法的位置。
>被执行的代码具有与该执行环境相同的作用链。
>这意味着通过eval()执行的代码可以引用在包含环境中定义的变量。

```
var msg = "hello world";
eval("alert(msg)");
```

在eval()中创建的任何变量或函数都不会被提升，
因为在解析代码的时候，他们被包含在一个字符串中；他们只在eval()执行的时候创建。

```
严格模式下，在外部访问不到eval()中创建的任何变量或函数，
因此前面的例子都会导致错误。同样，在严格模式下，为eval赋值也会导致错误。
```

能够接受代码字符串的能力非常强大，但也非常危险。
因此在使用eval()时必须极为谨慎，特别是在用它执行用户输入数据的情况下。
否则，可能会有恶意用户输入威胁你的站点或应用安全的代码（即所谓的代码注入）

#### 3、window对象

在全局作用域中生命的所有变量和函数，就都成为了window对象的属性。

在没有给函数明确指定this值的情况下，this值等于Global对象。

```
取得Global对象的方法：
var global = function(){
    return this;
}();
```

>这种语法格式为IIFE。
>IIFE是立即执行的函数表达式。
>语法格式：
>**(function(){})();**

```
特点：
    1、他是立即执行的
    2、他必须是表达式。
    3、后面有分号（手动重点），否则不能正常执行
```

>应用：
>1、保持数据私有性，避免创建全局变量
>2、把私有全局数据存储于一个单例对象
>3、把全局数据放入一个方法中

#### 4、Math对象

**4.1、min()和max()方法**

可以**接收多个参数**

```
    向这两个方法传入数组的方法：
    把Math对象作为apply()的第一个参数，从而正确的设置this值。
    然后，可以将任何数组作为第二个参数。
    var max = Math.max.apply(Math,values);
```
    
**4.2、random()方法**

```
    此方法返回大于等于0小于1的一个随机数。
    0=<Math.random()<1
```

