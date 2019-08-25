---
layout:     post
title:      JavaScript事件处理程序
subtitle:   JavaScript
date:       2019-08-25
author:     listen_spacer
header-img: img/background1.jpg
catalog: true
tags:
    - JavaScript
---
## JavaScript事件处理程序
1、DOM0级事件处理程序
```
var btn = document.getElementById("myBtn");
    btn.onclick = function(){
        alert("clicked");
    }  
```
>
>    只能一种事件只能绑定一个事件。
>    btn.onclick = null;        //删除事件处理程序
>
>    优点：简单、具有跨浏览器的优势

2、DOM2级事件处理程序
```
var btn = document.getElementById("myBtn");
    btn.addEventListener("click",function(){
        alert(this.id);
    },false)
```
>    
>    “DOM2级事件”定义了两个方法，用于处理指定和删除事件处理程序的操作：addEventListener()和removerEventListener()。
>
>    他们接收三个参数：要处理的事件名，作为事件处理程序的函数和一个布尔值。
>    最后这个布尔值参数如果是true，表示在捕获阶段调用事件处理程序；
>    如果是false，表示在冒泡阶段调用事件处理程序。
>
>    通过 addEventListener() 添加的事件处理程序只能使用removeEventListen()来移除。
>    **移除时传入的参数与添加处理程序时使用的参数相同。**
>    **这也就意味着通过addEventListener()添加的匿名函数将无法移除。**

3、IE事件处理程序
```
var btn = document.getElementById("myBtn");
    btn.attachEvent("onclick",function(){
        alert('Clicked');
    });
```
>
>    由于IE8及更正版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段。
>
>    IE实现了与DOM中类似的两个方法：attachEvent()和detachEvent()。
>    这两个方法接受相同的两个参数：事件处理程序名称与事件处理程序函数。
>
>    **注意，attachEvent()的第一个参数是"onclick"，而非DOM的addEventListener()方法中的"click"。**
>
>    在IE中使用attachEvent()与使用DOM0级方法的主要区别在于事件处理程序的作用于。
>    在使用DOM0级方法的情况下，事件处理程序会在其所属元素的作用域内运行；
>    在使用attachEvent()方法的情况下，事件处理程序会在全局作用域中运行，因此this等于window。
>
>    **在使用attachEvent()，为同一个元素添加两个事件处理程序的时候，执行顺序是最近添加的先执行。**
>
>    **支持IE事件处理程序的浏览器有IE和Opera。**