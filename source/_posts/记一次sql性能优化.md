---
title: 记一次sql性能优化
date: 2019-08-30 10:49:59
tags: 
- 调试
- mysql
cover: http://static.nnnnzs.cn/upload/bing/5ddb46aa2789176ff078a4b78ec39b04.png
---

有个小需求，在表里根据页码和页数查询，并排序，很简单，3秒就写出的sql
```mysql
select * from newslist order by ID desc limit ${(currPage - 1) * pageSize},${pageSize} ;
```
前几页测试没什么问题，很快
0.2s，在接收范围之内，然后试了一下最后一页
？？？
```mysql
select * from newslist order by ID desc limit 538070,10;
```
直到chrome的http请求超时了，都没有返回，跑到navicat里面一跑，90多秒
