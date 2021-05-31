---
layout: wiki
title: docker 基础
categories: wiki docker
description: docker 基础
keywords: docker
---

## 基本概念
+ [Docker](https://www.docker.com) 是一个基于 Go 语言的开源的应用容器引擎，允许高度可移植的工作负载，便于开发，交付和运行应用程序，具有可移植性和轻量级的特性。从 17.03 版本之后分为 CE（Community Edition: 社区版） 和 EE（Enterprise Edition: 企业版）
+ Docker 镜像是由文件系统叠加而成(是一种文件的存储形式)，其中包含应用程序及其依赖。通过这个文件，可以生成 Docker 容器。同一个 image 文件，可以生成多个同时运行的容器实例。

## 安装
> 参考 [菜鸟教程](https://www.runoob.com/docker/docker-tutorial.html)

使用官方安装脚本自动安装：
```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```
如果要使用 Docker 作为非 root 用户（即避免每次使用 docker 时都需要输入 sudo ），则应考虑使用类似以下方式将用户添加到 docker 组，注意执行后须注销重新登录才能生效：
```shell
sudo usermod -aG docker your-user
```

## 简单使用
### 启动与停止
```shell
# 启动 docker
sudo service docker start
# 停止 docker
sudo service docker stop
# 重启 docker
sudo service docker restart
```
### 镜像管理
```shell
# 列出镜像，其中 Tag 用于区分不同镜像
docker images
# 搜索镜像
docker search django
# 拉取镜像
docker pull hackeryx/ubuntu:16.04
# 删除镜像
docker image rm 镜像名或镜像id
# 镜像打包与加载
docker save -o 保存的文件名 镜像名
docker load -i 保存的文件名
```
### 容器操作
```shell
# 通过镜像创建容器
docker run [option] 镜像名 [向启动容器中传入的命令]
# 进入已运行的容器
docker exec -it 容器名或容器id 进入后执行的第一个命令 # 例如 /bin/bash
# 列出本机所有容器，包括已经终止运行的
docker ps -a

# 停止一个已经在运行的容器
docker container stop 容器名或容器id
# 启动一个已经停止的容器
docker container start 容器名或容器id
# kill掉一个已经在运行的容器
docker container kill 容器名或容器id

# 删除容器
docker container rm 容器名或容器id
# 将容器保存为镜像
docker commit 容器名 镜像名
```