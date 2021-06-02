---
title: shell编程
categories:
  - 运维
  - shell
tags:
  - shell
author:
  - Jungle
date: 2021-05-27 14:55:52
---

# 引言

**只记录对个人比较重要的命令！**

[参考链接](https://www.zutuanxue.com/home/4/6_20)

![image-20210527151703519](shellLearningNote/image-20210527151703519.png)

![image-20210527152002481](shellLearningNote/image-20210527152002481.png)

# curl 命令

[参考链接](http://www.ruanyifeng.com/blog/2011/09/curl.html)

1. 将docker容器提供的resnet50图像分类服务保存到输出文件

```shell
curl -X POST http://192.168.94.112:8080/predictions/resnet50 -T $1 -o output.txt
```

2. 输出文件内容：

![image-20210527152545975](shellLearningNote/image-20210527152545975.png)



# shell 脚本编程























