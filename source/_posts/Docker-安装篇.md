---
title: Docker-安装篇
date: 2020-01-05 14:30:46
tags: 路子越走越偏 docker
keyword: docker
---
## what is Docker
Docker 是一个开源的应用容器引擎
换句话说就是运行的系统里的虚拟机，可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
优点是容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

## Docker的应用场景
1. Web 应用的自动化打包和发布。
2. 自动化测试和持续集成、发布。
3. 在服务型环境中部署和调整数据库或其他的后台应用。
4. 从头编译或者扩展现有的 OpenShift 或 Cloud Foundry 平台来搭建自己的 PaaS 环境。

## Docker 的优点
1. 快速，一致地交付您的应用程序
2. 响应式部署和扩展
3. 在同一硬件上运行更多工作负载

## docker run hello-word
1. 在centos安装docker,非干净的环境卸载docker
``` bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

2. 设置仓库，安装所需软件
安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。并不稳定，参考[更换yum源](https://www.cnblogs.com/zrxuexi/p/11587173.html)
添加docker-ce的稳定镜像,
安装最新版本的docker-ce docker-ce-cli
``` bash
sudo yum install -y yum-utils  device-mapper-persistent-data  lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io
```
3. 更换docker镜像源
进入/etc/docker
如有，则改写，若无则创建daemon.json,写入如下内容
```json
{
    "registry-mirrors":["https://i5hfypef.mirror.aliyuncs.com"]
}
```
完成后重载
关于systemctl，可以参考[阮一峰Systemd](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)
```bash
systemctl daemon-reload
systemctl docker reload

```

4. 启动docker

```
sudo systemctl start docker
sudo docker run hello-world
```
第一次没找到，docker会自动找最新的image，然后安装，成功之后再次运行明令，就OK了

