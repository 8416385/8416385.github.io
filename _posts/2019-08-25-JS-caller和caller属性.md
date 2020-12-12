---
layout: post
title: JS-caller和caller属性
subtitle: caller和caller指针
date: 2019-08-25
author: listen_spacer
header-img: img/background3.jpg
catalog: true
tags:
  - JavaScript
---

## caller 和 caller 属性

##### 一、callee 属性是一个指针，指向拥有这个 arguments 对象的函数。

为什么要有这个属性呢？我们来看一下下面这个递归函数

```
	function factorial(num){	
		if(num<=1){
			return 1;
		}else{
			return num*factorial(num-1);
		}
	}
```

>     这个函数的执行和函数名factorial紧紧耦合在了一起。
>     当函数名发生改变的时候，里面的factorial也要随着改变
>     为了消除这种紧密耦合的现象，可以像下面这样使用arguments.callee

    function factorial(num){
    	if(num<=1){
    		return 1;
    	}else{
    		return num*arguments.callee(num-1);
    	}
    }

>     在这个重写的factorial()函数的函数体内，没有再引用函数名factorial。
>     这样，无论引用函数时使用的是什么名字，都可以保证正常完成递归调用。

```
例如：
    // 实际上是在另一个位置上保存了一个函数的指针
    var trueFactorial = factorial;

    factiroal = function(){
        return 0;
    }

    alert(trueFactorial(5))		// 120
    alert(factorial(0))			// 0
```

>     函数的名字仅仅是一个包含指针的变量而已。
>     function sayColor(){
>     	alert(this.color);
>     }

>     o.sayColor = sayColor;
>     全局的sayColor()函数与o.sayColor()指向的仍然是同一个函数。

##### 二、ECMAScript5 也规范了另一个函数对象的属性：caller

>     这个属性中保存着调用当前函数的函数引用，如果是在全局作用域中调用当前函数，它的值为null。

```
	例如：
		function outer(){
			inner();
		}
		function inner(){
			alert(inner.caller);
		}

		outer();
	这是提示框中显示的是out()函数的源代码，
	因为outer()调用了inner()，所以inner.caller就指向outer()。
```

>     严格模式下：
>     	访问arguments.callee会导致错误。
>     	ECMAScript5还定义了arguments.caller属性，
>     	严格模式下访问也会导致错误，非严格模式下这个属性始终是undefined。

> 严格模式下，不能为函数的 caller 属性赋值，否则会导致错误。

> 定义 arguments.callee 属性是为了分清 arguments.caller 和函数的 caller 属性。
> 参数用 callee，函数用 caller
