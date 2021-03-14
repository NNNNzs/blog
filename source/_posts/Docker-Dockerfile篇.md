---
title: docker-Dockerfile篇
date: 2020-01-16 09:51:55
tags: docker
keyword: docker
cover: https://static.nnnnzs.cn/bing/20200116.png
---

## 什么是Dockerfile
Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明,相当于快速构建时候的脚本文件

## 获取images
1. 首先明确需要获取的image版本，以目前LTS的nodejs版本 12.14.0为例，在宿主机pull对应的版本，这一步不做也没关系，Dockerfile会自动拉
```bash
docker pull node:12.14.0
```
## 编写需要运行的node程序
1. 先确定需要的依赖包，编写package.json文件，需要注意锁版本，生产和发布可能会因为时间不同，安装了新版本会有不可控的问题
```
{ 
    "name": "dockerTest", 
    "version": "1.0.0",
    "description": "Node.js on Docker", 
    "author": "NNNNzs",
    "main": "server.js",   
    "scripts": {
        "start": "node server.js"
    },
    "dependencies": { 
        "express": "4.13.3" 
    }
  }
```
2. 编写运行的javascript服务，主要功能就是首页输出HelloWord
```javascript
    'use strict';
    const express = require('express');
    const PORT = 8888;
    const app = express();
    app.get('/', function (req, res) { 
        res.send('Helloworld\n');
    });
    app.listen(PORT);
    console.log('Running on http://localhost:' + PORT);
```

### 编写Dockerfile
1. Dockerfile的指令都市大写，依赖的构建镜像的基础源镜像必须在第一行
2. 在容器中创建一个目录，-p是递归创建，如果父目录不存在则创建父目录
3. 切换工作目录区到Service
4. 将当前目录的文件拷贝到Service
5. 安装nodejs的依赖,用淘宝的源
6. 暴露端口给宿主机
7. 构建完成启动服务
```bash
    FROM node:12.14.0
    RUN mkdir -p /home/Service
    WORKDIR /home/Service
    COPY . /home/Service
    RUN npm install --registry=https://registry.npm.taobao.org
    EXPOSE 8888
    CMD npm start   
    ## 如果想运行多条指令可以这样：
    ## CMD git pull && npm install && npm start
```
### 构建Image
1. 首先需要切换到Dockerfile和文件所在的文件夹，再运行打包命令
```
docker build -t mynodeapp .
```
2. -t name 表示镜像名
3. 最后结尾的 . 表示当前目录的文件夹

### 运行镜像
```bash
docker run -d -p 80:8888  --name node-test mynodeapp
```
1. -d 表示后台运行
2. -p 表示端口映射 宿主机80映射到container 8888端口
3. --name node-test 表示运行container的实例名
4. mynodeapp表示运行的镜像名 

### 检查是否正常运行
```bash
docker ps -a 
```

### 在宿主机查看80端口是否正常返回
```bash
curl localhost:80
```
正常返回helloword就OK了，具体上外网可能还需要宿主机暴露端口

