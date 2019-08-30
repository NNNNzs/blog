---
title: 花式log
date: 2019-08-28 20:26:09
cover: http://static.nnnnzs.cn/upload/bing/20d521ee7c1f84c3430c02513401061d.png
tags: 
- 调试
- JavaScript
---
# 各种控制台输出
## 常见的控制台打印
```Javascript
console.log()    // 打印日志
console.debug()  // 打印调试
console.error()  // 打印错误
console.info()   // 打印信息
console.warn()   // 打印警告
console.clear()  // 清空


```
## 调试类控制台
```Javascript
console.count('count') //count被调用了多少次计数，
console.memory //是个属性，输出内存
console.profile('Test Profile') //性能检测开始
console.profileEnd('Test Profile') //性能检测结束

console.time('test time')//开始计时
console.timeEnd('test time')//结束计时

console.assert(flag)//如果flag为假，报异常
console.trace()//用来追踪函数的调用轨迹。
```
## 格式化输出

```javascript
console.log('%d', 123); //%d表示数字
console.log('%i', 123); //%i表示整型数字
console.log('%f', 123.321); //%f表示浮点型
console.log('%o', document.body); //%o表示DOM元素
console.log('%O', new Date()); //%O表示javascript对象
console.log('你好，我是%s,.我的博客地址是%s','NNNNzs','https://blog.nnnnzs.cn')

console.table()  // 数组或对象以表格形式打印

//分组打印
console.group("第一组信息");
console.log("第一组第一条");
console.log("第一组第二条");
console.groupEnd("第一组信息");
```
### 输出样式
```Javascript
console.log('%cNNNNzs', 'color: #21759b;font-size: 50px;font-weight: bold;display:inline-block;');
```

<script>        
        console.log('%cNNNNzs', 'color: #21759b;font-size: 50px;font-weight: bold;display:inline-block;');
</script>