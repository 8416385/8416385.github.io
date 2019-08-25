---
layout:     post
title:      JS-JavaScript数据属性和访问器属性
subtitle:   JavaScript
date:       2019-08-25
author:     listen_spacer
header-img: img/background1.jpg
catalog: true
tags:
    - JavaScript
---
## JavaScript数据属性和访问器属性
ECMAScript中有两种属性：**数据属性**和**访问器属性**

#### 1、数据属性

>数据属性包含一个数据值的位置。在这个位置可以读取和写入值。
>数据属性有4个描述其行为的特性，默认值都是true

**1.1、configurable**

>    表示能否通过delete删除属性从而重新定义属性，能否修改属性特性，或者能否把属性修改为访问器属性

**1.2、enumerable**

>    表示能否通过for-in循环返回属性。

**1.3、writable**

>    表示能否修改属性的值


**1.4、value**
>包含这个属性的属性值

```
要修改属性默认的特性，必须使用ECMAScript5的Object.defineProperty()方法
这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象
例如：
Object.defineProperty(person,"name",(
    writable:false,
    value : "Nicholas"
))
```

>**一旦把属性定义为不可配置的，就不能再把它变回可配置了。**
>此时，再调用**Object.defineProperty()** 方法修改除 **writable** 之外的特性，都会导致错误
>（建议不要再IE8以下的版本使用Object.defineProperty()方法）

#### 2、访问器属性

访问器属性**不包含数据值**。

**2.1、configurable**
>表示能否通过**delete**删除属性从而重新定义属性，能否修改属性特性，
>或者能否把属性修改为数据属性

**2.2、enumerable**
>[ ] 表示能否通过for-in循环返回属性。

**2.3、get**
>在读取属性时调用的函数。默认值为undefined

**2.4、set**
>在写入属性时调用的函数。默认值undefined

**访问器属性不能直接定义，也需使用Object.defineProperty()来定义。**

_year
**下划线** 是一种常用的记号，用于表示只通过对象方法访问的属性

需要指定set或者get方法，如果只指定一个的话，属性会变成只读或者只可写入

```
定义多个属性：

    ECMAScript 5有定义了一个Object.defineProperties()方法，
    利用这个方法可以通过描述符一次定义多个属性，
```

>这个方法接收两个参数：
>要添加和修改其属性的对象，对象的属性与要修改或添加的属性
>这里的属性都是在同一时间创建的

**支持Object.defineProperties()方法的浏览器有IE 9+、Firefox 4+、Safari 5+、Opera 12+和Chrome**

**读取属性的特性：**

>使用ECMAScript 5的Object.getOwnPropertyDescriptor()方法，
>可以取得给定属性的描述符。
>
>这个方法接受两个参数：
>
>属性所在的对象和要读取其描述符的属性名称，返回值是个对象
>
>var descriptor = Object.getOwnPropertyDescriptor(book,"_year");
>通过接收返回值的方式，获取属性

```
对于数据属性取出的对象，value等于最初的值，
configurable是false，而get等于undefined。

对于访问器属性，value等于undefined，
enumerable是false，而get是一个指向getter函数的指针
```