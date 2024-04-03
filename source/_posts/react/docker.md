---
title: docker入门总结
categories: 
    - 服务器
tags: 
    - docker
    - 部署
    - 容器
cover: /static/images/docker.jpg
---

## 安装

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

## 查看容器

```shell
docker container ls  #正在运行的
docker container ls --all  #所有的
```

## 拉取镜像

```shell
docker pull [容器名]
```

## 制作容器

1、新建Dockerfile文件

```shell
FROM node:8.4 #继承node8.4

COPY . /app # 将当前目录下的所有文件（除了.dockerignore排除的路径），都拷贝进入 image 文件的/app目录。

WORKDIR /app #指定接下来的工作路径为/app。

RUN npm install --registry=https://registry.npm.taobao.org  #在/app目录下，运行npm install命令安装依赖。

EXPOSE 3000 #导出3000端口
```

2、创建image文件

```shell
docker image build -t koa-demo . # -t指定image的名字 后面的点表示Dockerfile文件目录
```

## 启动容器

```shell
docker container run -d -p 8000:3000 -it koa-demo /bin/bash
```

1、常用命令

```
-d: 后台运行容器
-p： 容器的 3000 端口映射到本机的 8000 端口。
-it：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。
koa-demo:0.0.1：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）。
/bin/bash：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell。
```

[更多](https://docs.docker.com/engine/reference/commandline/run/)

## 其他命令

```shell
docker container start [containerID] #启动已存在的容器
docker container stop  [containerID] #停止容器
docker container logs [containerID]  #查看标准输出
docker container cp [containID]:[/path/to/file] . #拷贝容器内的文件到本机当前目录
docker exec -it [containID] /bin/bash #进入容器 bash没有的话，可以换成sh
```



## 文档

[数据管理 - Docker —— 从入门到实践 (gitbook.io)](https://yeasy.gitbook.io/docker_practice/data_management)