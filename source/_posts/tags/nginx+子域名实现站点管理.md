---
title: nginx + 子域名实现站点管理
date: 2020-12-05 17:13:45
categories: 
    - 服务器
tags: 
    - nginx
    - 部署
cover: /static/images/nginx.jpg
typora-root-url: ..\..
---

## 主要思路

1、先利用统配域名把所有的解析都指向装有nginx的主机

2、主机上分别再不同的端口运行多个项目

3、nginx通过监听host，代理到不同的端口

## 安装nginx

```shell
1、rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm #如果没有源，先添加yum源
2、yum install -y nginx #安装
3、nginx -v #检查是否安装成功
4、nginx -t #查看配置文件目录
5、vim /etc/nginx/nginx.config #编辑配置文件
```

## 在服务器上运行程序

1、如果是静态项目，直接放在一个文件夹放着就行

2、vue项目的话，把build后的dist文件夹传上来

3、springboot项目可以用java -jar xxx.jar &将打包后的jar包跑起来

4、也可以在docker中把项目跑起来，记下映射端口

## 代理

添加一个server

```shell
# 需要运行的项目
server {
    listen       80; 
    server_name  mall.lhl94666.cn; #监听这个地址
    location / {
    	proxy_pass http://localhost:28089; #就是你程序跑起来的那个端口
    }
}
```

```shell
# 静态项目
server {
    listen    80;
    server_name portal.lhl94666.cn;
    location / {
         root   /opt/demo/portal; #网站根目录
         index  index.html; #入口文件
         try_files $uri $uri/ /index.html;
    }
}
```

## 重启nginx

```
nginx -s reload #重启nginx
```

浏览器输入你刚刚配置的mall.lhl94666.cn就能看到效果啦~