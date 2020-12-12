---
layout: post
title: JS-浅谈Web Worker
subtitle: 随便聊聊Web Worker
date: 2020-12-12
author: listen_spacer
header-img: img/bg/2.jpg
catalog: true
tags:
  - JavaScript
---

Web Worker通常应用于哪些方面呢：
1、处理密集型数学计算
2、大数据集排序
3、数据处理（压缩、音频分析、图像处理等）
4、高流量网络通信

Worker是浏览器提供的异步线程。在Worker内部是无法访问主程序的任何资源的。这意味着你不能访问它的任何全局变量，也不能访问页面的DOM或者其他资源。记住，这是一个完全独立的线程。

但是，你可以执行网络操作以及设定定时器。还有，Workder可以访问几个重要的全局变量和功能的本地复本，包括navigator、location、JSON和applicationCache。

Worker  w1 对象是一个事件侦听者和触发者，可以通过订阅它来获得这个 Worker 发出的事
件以及发送事件给这个 Worker。

以下是如何侦听事件（其实就是固定的 "message" 事件）：

```
w1.addEventListener( "message", function(evt){
// evt.data
} );
```

也可以发送 "message" 事件给这个 Worker：

w1.postMessage( "something cool to say" );

在这个 Worker 内部，收发消息是完全对称的：
```
// "mycoolworker.js"
addEventListener( "message", function(evt){
// evt.data
} );
postMessage( "a really cool reply" );
```