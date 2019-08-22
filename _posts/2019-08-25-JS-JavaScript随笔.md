---
layout:     post
title:      JS-JavaScript随笔
subtitle:   JavaScript
date:       2019-08-25
author:     listen_spacer
header-img: img/background1.jpeg
catalog: true
tags:
    - JavaScript
---
```
	contenteditable属性

		设置控件是否可以编辑，默认为false
		适用于所有便签，不仅限于p标签，a标签
		HTML5中加入的新属性


	querySelector()方法

		利用css选择器进行查找控件。
		没有getELementById()方法的局限性。


	onclick="函数"与控件.addEventListener()的区别

		onclik的这种形式侵入了HTML，耦合高，后期修改麻烦，尽量避免使用这种形式。


	控件.setAttribute("属性",值);

		获取或修改控件的属性


	preventDefault()方法

		阻止控件的默认动作/功能


	ES6 增强型for循环

		for(one of li){
		}


	js绑定监听事件的三种方发：

		1、<a onclick="link()"></a>
		2、控件.onclick(function(){})
		3、控件.addEventListener("click",function(){})
		前两种绑定的方式，无法解除事件绑定，
		而第三种可以，控件.removeEventListener("事件",函数名)。


	字符串（注意事项）：

		在JavaScript中字符串是固定不变的，类似replace()和toUpperCase()方法都返回新字符串，原字符串本身并没有发生改变。
		返回的只是一个副本，本身并没有任何改变。

		ECMAScript 5中，字符串可以当作只读数组，
		除了使用charAt()方法，也可以使用方括号来访问字符串中的单个字符

		字符串既然不是对象，为什么它会有属性呢？

			当引用了字符串的属性时，
			JavaScript就会将字符串通过new String(s)的方式转换成对象，
			一旦属性引用结束，这个新创建的对象就会销毁
			（其实在实现上并不一定创建或销毁这个临时对象，然而整个对象看起来是这样）

			可以给字符串、数字和布尔值赋属性，但赋完就被销毁了，取不到这个值。


	对象转换为原始值：

		所有对象继承了两个转换方法：toString()，valueOf()

				toString()					valueOf()
		对象	[object object]				返回对象本身
		日期	可读的日期和时间的字符串	  1970年1月1日的毫秒数

		对象到字符串（object-to-string）:

			1、如果对象具有toString()方法，则调用这个方法。如果返回了一个原始值，则将这个原始值转换为字符串返回。
			2、如果没有toString()方法，或者toString()返回的并不是一个原始值，则会调用valueOf()方法。
			3、否则，JavaScript无法从toString()或valueOf()获得一个原始值，因此这时他将抛出一个类型错误异常。

		对象到数字（object-to-number）

			与对象到字符串相反。先调用valueOf()，再调用toString()方法。
			两个方法都没有获得原始值的话，也会抛出一个类型错误异常。


		函数作用域：

			在类似C语言的编程语言中，花括号内的每一段代码都具有各自的作用于，而变量在声明它们的代码段之外是不可见的，我们称之为块级作用域。

			而JavaScript中没有块级作用域。取而代之的使用了函数作用域。

			JavaScript的函数作用域是指在函数内生命的所有变量在函数体内始终是可见的。

			有意思的是，这意味着变量在生命之前甚至已经可用了。
			JavaScript的这一个特性被非正式地称为声明提前（hoistiong）
			JavaScript函数里生命的所有变量（但不涉及赋值）都被“提前”至函数体的顶部。

			“声明提前”这步操作是在JavaScript引擎的“预编译”时进行的，是在代码开始运行之前。


			var scope = "global";		由于JavaScript的声明提前	                        var scope = "global";
			function f(){						等价于				        fucntion f(){
				console.log(scope);			=============>		                var scope;
				var scope = "local";									                console.log(scope);
				console.log(scope);										        var scope = "local";
			}															console.log(scope);
																	}
			因此第一个console.log并不是想象中的global，而是undefined

			将变量声明放在函数体顶部，而不是将声明靠近放在使用变量之处。
			这种做法使得源代码非常清晰地反映了真实的变量作用域。

			Javascript全局变量是全局对象的属性。
			因此可以通过 delete 变量名 删除
			而通过var定义的局部变量则不可以。	
```