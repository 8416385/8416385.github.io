---
layout:     post
title:      vue——class与style绑定
subtitle:   VUE，样式动态绑定
date:       2020-12-12
author:     listen_spacer
header-img: img/bg/8.jpg
catalog: true
tags:
    - VUE
---

#### v-bind及class与style绑定
v-bind的主要用法是动态更新HTML元素上的属性

##### 绑定class的几个方式
###### 1、使用v-bind动态绑定class

```
<div :class="{ 'active' : isActive }"</div>
```
如果想要给class赋予固定的class，可以class='class1'或者:class="'class1'"
class:表达式，当表达式为真的时候，对应的类名才会被加载


###### 2、class还可以绑定一个计算属性
```
var app = new Vue({
    el: '#app',
    data: {
        isActive: true,
        error: null
    },
    computed: {
        classes: function () {
            return {
                active: this.isActive && !this.error,
                'textfail': this.error && this.error.type === 'fail'
            }
       }
    }
})
```
除了计算属性，也可以直接绑定一个Object类型的数据，或者使用类似计算属性和methods

###### 3、数组语法
当需要应用多个class时，可以使用数组语法，给:class绑定一个数组，应用一个class列表

```
<div :class="[activeCls,errorCls]"></div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            activeCls: 'active',
            errorCls: 'error'
        }
     })
 </script>
 渲染后的结果:
 <div class="active error"></div>
```
使用三元表达式来根据条件切换class

```
<div :class="[isActive ? activeCls : '', errorCls]"></div>
class有多个条件时，这样写较为烦琐，可以在数组语法中使用对象语法：
<div :class="[{ 'active': isActive }, errorCls]"></div>
```
