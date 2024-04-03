---
title: linux 常用命令
categories:
  - 服务器
tags:
  - linux
  - 部署
  - 容器
cover: /static/images/docker.jpg
---

#### 几个常用的命令

###### 生成公钥

```
ssh-keygen
```

###### 上传公钥到服务器

```
ssh-copy-id -i ~/.ssh/id_rsa.pub 用户名@主机名
```

###### SCP 上传文件夹示例

```
scp -r -P 22 ./dist/* bingli@139.199.182.28:/home/bingli/code/deploy/all-assert/report_distribute/
```

###### 安装微软开发套件

```shell
npm install --global --production windows-build-tools
```

###### ssh 隧道-端口转发

```shell
ssh -p 2789 -L 3306:localhost:3306  bingli@101.33.225.202
```

###### 设置代理

```shell
export http_proxy=http://192.168.1.90:7890
export https_proxy=http://192.168.1.90:7890

#取消
unset http_proxy
unset https_proxy
```

###### 解压 asar

```
asar extract app.asar ./myapp
```

###### 端口占用

```
lsof -i tcp:8888
```

###### 查看 xxx 的运行情况

```
ps aux | grep xxx
netstat -tuln | grep 8080

OLLAMA_HOST=0.0.0.0:8080 ollama serve
```

###### syetemctl

​ 在 Linux 系统中，/lib/systemd/system 目录是 systemd 服务管理系统的主要配置文件目录。这个目录下存储了各种服务的 .service 文件，这些文件定义了服务的启动参数。
每一个 .service 文件都对应一个服务，文件里面包含了服务的描述、运行的命令、依赖关系等各种信息。
​ 当我们使用 systemctl enable [服务名称] 命令设置一个服务开机自启动时，系统会在 /etc/systemd/system 目录下创建一个到 /lib/systemd/system/[服务名称].service 的符号链接。
​ 这样，无论你是要启动、停止、还是自启动一个服务，systemd 都会根据 /lib/systemd/system 目录下的对应 .service 文件来操作。

```
systemctl start [服务名称]：启动一个服务。例如：systemctl start jenkins。
systemctl stop [服务名称]：停止一个服务。例如：systemctl stop jenkins。
systemctl restart [服务名称]：重启一个服务。例如：systemctl restart jenkins。
systemctl reload [服务名称]：如果服务支持，可以使用此命令重新加载服务的配置文件，而无需重启服务。
systemctl status [服务名称]：查看一个服务的状态。例如：systemctl status jenkins。
systemctl enable [服务名称]：设置一个服务开机自启动。例如：systemctl enable jenkins。
systemctl disable [服务名称]：关闭一个服务的开机自启动。例如：systemctl disable jenkins。
systemctl list-units --type=service --all：列出所有的服务单元。
systemctl reboot：重新启动系统。
systemctl poweroff：关闭系统。
```
