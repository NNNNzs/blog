---
title: 记一次sql性能优化
date: 2019-08-30 10:49:59
updated: 2019-08-30 21:38:25
tags: 
- 调试
- mysql
cover: //static.nnnnzs.cn/upload/bing/20190830.png
---

有个小需求，在表里根据页码和页数查询，并排序，很简单，3秒就写出的sql
```sql
select * from newslist order by id desc limit ${(currPage - 1) * pageSize},${pageSize} ;
```
前几页测试没什么问题，很快
0.2s，在接收范围之内，然后试了一下最后一页
？？？
```mysql
select * from newslist order by id desc limit 538070,10;
```
直到chrome的http请求超时了，都没有返回，跑到navicat里面一跑，90多秒
查阅相关资料，会全表扫描，但是丢弃前538069页的内容

```sql
SELECT * FROM (select id from newslist limit ${(currPage - 1) * pageSize},${pageSize}) as a,newslist as n WHERE a.id = n.id  
```
看似没有问题了，先根据唯一索引查询id，再根据id查询，但是又出问题
子查询查询的内容不是竟不是从1开始的

```sql
select id,title from newslist limit ${(currPage - 1) * pageSize},${pageSize}
select * from newslist limit ${(currPage - 1) * pageSize},${pageSize}
```
查询所有内容，查询主键id，和另外一个列的时候，排序结果又正常了
可能是当时建表的时候，id没有直接主键，插了几条数据再设置主键
网上有言论是存储时间不同，导致存在硬盘上的地址不连续云云。。。
聚集索引云云的
