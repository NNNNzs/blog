---
title: 记一次js页面跳转
date: 2019-08-28 16:42:40
tags: 
- jQuery
cover: https://static.nnnnzs.cn/upload/bing/0b7364e230713f8707e9f0b1d56c6e10.png
---
碰到一个需求，软件站的某个页面想要用js做404跳转，但是不想删除cms后台的软件，还想做SEO，不然降低SEO权重
要求是从搜索引擎来的，页面源码还是原来的，但是展示404
直接访问地址URL，跳转真实的404页面

根据页面标题，包含违禁词跳转
根据来路，做真实的404还是假的404

```javascript

function direct() {
    var illegalKey = ['彩票', '赌博'];
    var title = document.title;
    var where = document.referrer;//来路地址
    var regexpp = /\.(sogou|so|baidu|360|google|sm|yahoo|bing)(\.[a-z0-9\-]+){1,2}\//ig;
    var isFromSeo = regexpp.test(where);
    var isIllegal = false;
    for (var i = 0; i < illegalKey.length; i++) {
        if (title.indexOf(illegalKey[i]) > -1) {
            isIllegal = true
            break;
        }
    }
    if (isIllegal) {
        if (isFromSeo) {
            $("title").html('您访问的页面不存在');
            window.stop ? window.stop() : document.execCommand("Stop");
            $("body").load("/404.html")
        } else {
            window.location.href = "/404.html"
        }
    }
}
```
比较有意思的就是jquery的load函数，能让body展示某个页面的样式，但是不改变源码
查看相关文档
```javascript
$(selector).load(url,data,function(response,status,xhr))
```

|参数|描述
|:-----  |:-----
|url|必需。规定您需要加载的 URL。
|data|可选。规定连同请求发送到服务器的数据。
|function(response,status,xhr)|可选。规定 load() 方法完成时运行的回调函数。<br>额外的参数：<br>response - 包含来自请求的结果数据<br>status - 包含请求的状态（"success"、"notmodified"、"error"、"timeout"、"parsererror"）<br>xhr - 包含 XMLHttpRequest 对象

还是有点意思的
