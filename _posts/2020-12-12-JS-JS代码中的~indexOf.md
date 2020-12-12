---
layout: post
title: JS-JS代码中的~indexOf
subtitle: 聊些有趣的东西
date: 2020-12-12
author: listen_spacer
header-img: img/bg/2.jpg
catalog: true
tags:
  - JavaScript
---
在维护老代码或者逛社区的时候，你也许也会见过~indexOf的写法。

```
a.indexOf('b') > -1 ==> ~a.indexOf('b')
```

那么为什么呢？学过计算机组成原理的朋友应该知道补码和反码的概念。
~字位取反 也就是计算机科学中的反码

> 0的补码为-1
> -1的反码就是-0

有些观点认为，用indexOf>-1或者indexOf==-1，这种写法很不好，称为“抽象渗漏”;

> 意思就是在代码中暴露了底层的实现细节。这些细节应该被屏蔽掉。

~和indexOf()一起可以将结果强制转换为真/假值。

只有在indexOf返回-1时，~indexOf才会被转换为假（-0）。
