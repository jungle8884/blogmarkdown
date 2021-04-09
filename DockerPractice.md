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



# 搭建Tensorflow2-GPU

## 1、运行容器：portainer

```shell
docker run -d -p 80:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true --name portainer portainer/portainer
```

## 2、运行容器：tf2gpu

```shell
jungle@vr:~$ docker run -it --name=tf2gpu --runtime=nvidia -v /home/jungle/data:/home/jungle/data 8f81a42bf0d0
```

