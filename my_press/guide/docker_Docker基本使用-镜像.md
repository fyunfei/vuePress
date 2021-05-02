---
title: Docker基本使用-镜像
category: Docker
tag: -镜像
comment: false
---

# Docker 基本管理-镜像

## 查找镜像

**docker search keyword**

## 拉取镜像

**docker pull name:tag**

**name**表示我们当前拉取的镜像名称

**tag**表示指定镜像的版本，若没有指定则拉取最新版本即 latest

## 查看本地镜像

**docker images**

## 删除本地镜像

**docker rmi image**

## 查看镜像详情

**docker inspect**

## 镜像构建

镜像构建包括两种方法，一种是**docker commit**，另一种是**docker build**和**DockerFile 文件**

## 镜像标签管理

**docker tag source_image[:tag] target_image[:tag]**
我们可以理解为该命令创建一个源镜像的拷贝，两个镜像公用一个存储空间，只是镜像的 respository 不同或者 tag 不同。
