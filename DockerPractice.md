---
title: DockerPractice
categories:
  - 运维
  - Docker
tags:
  - Docker 
author:
  - Jungle
date: 2021-04-09 
---



# 搭建DL-GPU

## 1、运行容器：portainer

```shell
docker run -d -p 80:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true --name portainer portainer/portainer
```

## 2、运行容器：tf2gpu

```shell
jungle@vr:~$ docker run -it --name=tf2gpu --runtime=nvidia -v /home/jungle/data:/home/jungle/data 8f81a42bf0d0
```

## 3、运行容器：pytorch-jungle

```shell
docker run -it --name=pytorch-jungle --runtime=nvidia -v /home/jungle/data:/home/jungle/data f8a1d10ae3d7
```



# 测试DL

> 训练vgg-16

```shell
# use gpu to train vgg16
$ python train.py -net vgg16 -gpu
```

> 测试vgg-16

```shell
python test.py -net vgg16 -weights checkpoint/vgg16/Thursday_06_May_2021_05h_56m_01s/vgg16-200-regular.pth
```



## 遇到的问题

> RuntimeError: DataLoader worker (pid XXX) is killed by signal: Bus error （给运行中的容器修改共享内存）

解决办法：https://blog.csdn.net/u014090429/article/details/108199059

**如何预防**：docker run的时候加参数--shm-size=32212254720（32G=32*1024^3）

