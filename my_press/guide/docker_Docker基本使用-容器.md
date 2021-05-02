---
title: Docker基本使用-容器
category: Docker
tag:
  - 容器
comment: false
---
# Docker基本使用-容器
如果说镜像是一种静态的文件，那么容器一定是一种动态的存在。在Docker中，容器是提供服务的主体。管理好容器是系统管理员最主要的任务之一。

## 创建容器
创建容器有两种方式：
- docker create
- docker run

两者的区别就是前者创建容器不会立即启动，后者会立即启动容器。

### docker run
-t t表示terminal，表示为当前容器提供一个虚拟终端以便于用户与容器交互，这样的方式创建的容器成为**交互式容器**。

-d 表示创建的容器在后台运行，同时会输出这个容器的ID，通过这样的方式创建的容器称为**后台容器** 

## 查看容器
**docker ps**
- -a 表示列出全部容器
- -f 根据条件筛选容器
- -n 表示最后n个创建的容器
- -latest 最后创建的容器
- -q 只显示容器的id，可以与其他命令组合

## 启动/重启容器
**docker start container_id/container_name**

**docker restart container_id/container_name**

## 停止容器docker stop

## 删除容器docker rm