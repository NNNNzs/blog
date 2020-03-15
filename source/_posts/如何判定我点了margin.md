---
title: 如何判定我点了margin?
date: 2020-02-06 17:27:58
tags:
cover: https://static.nnnnzs.cn/bing/20200206.png
---
这个世界上唯有两样东西能让我们的心灵感到深深的震撼：一是我们头顶上灿烂的星云，二是我们心中崇高的道德法则。

one - 康德

## 前言
页面的绑定事件都是绑定在DOM元素上的，诸如此类
```javascript
document.getElementById('#app').addEventListener('click',function(e){
    // todo
})
```
最近接到一个需求，要求点击元素主体的是否没有反应，点击非展示内容的时候才有做出响应，
相信读者朋友读到这里大概能理解我做的是什么样的功能，就是判断点了元素的margin

``html


```
