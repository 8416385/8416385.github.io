---
layout: post
title: ElementUi 表单验证失败后，跳转到失败表单位置
subtitle: VUE
date: 2021/7/21
author: listen_spacer
header-img: img/bg/12.jpg
catalog: true
tags:
  - vue
  - ElementUI
  - 表单
---

#### 需求

> 开发中，偶尔会出现超长表单的情况。  
> 而一般我们的操作按钮都是在头部或者顶部悬挂。  
> **这时就会出现一个问题：当我们点击操作按钮时，部分表单元素不在浏览器视口中。**  
> 这时如果校验失败的表单元素，恰在其中的话。用户的体验就是：这个按钮我按了，没有反应，怎么回事  

#### 解决办法

##### 滚动到校验失败的第一个元素的表单位置

###### 实现思路

> vue.$refs 中存储着各个 vue 组件的实例。我们需要获取的元素位置信息也在这个 KeyMap 中。  
> 1、首先，我们通过 ref 属性，获取表单实例对象  

```
this.$refs[formRef]
```

2、获取到验证失败的表单元素
 
> 获取位置之前我们需要找到哪些表单验证失败了。  
> 在阅读了 **elementUI form** 相关代码之后，我们找到了 **fields** 属性，这个里面存储着 form 中所有字段实例  
> 而且，每个实例都拥有 **validateState** 属性，当该属性值等于 **error** 时，则代表验证失败。  
> 那么实现第二步的代码如下：

```
var errorElDoms = elDom.fields.filter((item) => {
    return item.validateState === 'error';
});
```

3、获取距顶最近的表单元素位置

> 既然我们拿到了 **验证失败实例数组** ，则可以获取每个实例距顶的位置。  
> 每个 errorElDom 里面，都有个属性 **$el** ，他的引用指向对应字段的 dom 实例。  
> 通过 **.$el.offsetTop** ，获取到所有距顶数值，然后进行 sort 排序，只取第一项。  

```
var errorOffsetTops = errorElDom
    .map((item) => {
        return item.$el.offsetTop;
    })
    .sort();
    return errorOffsetTops[0];
```

#### 完整代码

###### 至此，我们的功能已经完成了！

###### 完整代码如下：

```
// 页面滚到到指定位置
document.querySelector(
    '.el-scrollbar__wrap'
).scrollTop = self.getErrorFieldOffsetTop(self.$refs['userInfoFrom']);  // userInfoFrom —— elForm的ref值

/**@function 获取错误框的offsetTop */
getErrorFieldOffsetTop(elDom) {
    var errorElDom = elDom.fields.filter((item) => {
    return item.validateState === 'error';
    });
    var errorOffsetTops = errorElDom
    .map((item) => {
        return item.$el.offsetTop;
    })
    .sort();
    return errorOffsetTops[0];
}
```
