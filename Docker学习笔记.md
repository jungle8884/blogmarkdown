---
title: Docker学习笔记
categories:
  - Docker
tags:
  - docker
  - 镜像
  - 容器
author:
  - Jungle
date: 2021-03-19 15:27:15
---

# Docker的常用命令 #

## 启动Docker

- systemctl start docker

## 帮助命令 ##

- docker version		# 显示docker的版本信息
- docker info			# 显示docker的系统信息, 包括镜像和容器的数量
- docker 命令 --help 	# 帮助命令

## 镜像命令 ##
### docker images	查看所有本地主机上的镜像 ###

```shell
[lighthouse@VM-12-17-centos ~]$ docker images
REPOSITORY              TAG          IMAGE ID       CREATED         SIZE
tensorflow/tensorflow   latest-py3   53187075965b   14 months ago   2.52GB

**解释**

    - REPOSITORY	镜像的仓库源
    - TAG			镜像的标签
    - IMAGE ID		镜像的ID
    - CREATED		镜像的创建时间
    - SIZE			镜像的大小

**可选项**

     -a, --all             # 列出所有镜像
     -q, --quiet           # 只显示镜像的ID
```

### docker search 搜索镜像 ###

```shell
[lighthouse@VM-12-17-centos ~]$ docker search mysql
	NAME                              DESCRIPTION                                     STARS     OFFICIAL 
	mysql                             MySQL is a widely used, open-source relation…   10629     [OK]       
	mariadb                           MariaDB Server is a high performing open sou…   3987      [OK]       
	mysql/mysql-server                Optimized MySQL Server Docker images. Create…   779       [OK]      
```

#### 可选项 ####

```shell
-f, --filter=STARS=3000   搜索出来的镜像就是STARS大于3000的

	[lighthouse@VM-12-17-centos ~]$ docker search mysql --filter=STARS=3000
	NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
	mysql     MySQL is a widely used, open-source relation…   10629     [OK]       
	mariadb   MariaDB Server is a high performing open sou…   3987      [OK]    
```

### docker pull 下载镜像 ###

```shell
# 下载镜像 docker pull 镜像名[:tag]
[lighthouse@VM-12-17-centos ~]$ docker pull mysql
Using default tag: latest			# 如果不写 tag, 默认就是 latest
latest: Pulling from library/mysql
6f28985ad184: Pull complete  		# 分层下载: docker image的核心 联合文件系统
e7cd18945cf6: Pull complete 
ee91068b9313: Pull complete 
b4efa1a4f93b: Pull complete 
f220edfa5893: Pull complete 
74a27d3460f8: Pull complete 
2e11e23b7542: Pull complete 
fbce32c99761: Pull complete 
08545fb3966f: Pull complete 
5b9c076841dc: Pull complete 
460de37ce6f9: Pull complete 
9c38e51eabd2: Pull complete 
Digest: sha256:bfb6bdc172e101a3e7ab321f541bd4e3f9ac11bac8da7ea0708defcaf2c7554e # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest		# 真实地址

# 等价于它
docker pull mysql
docker pull docker.io/library/mysql:latest
```

### docker rmi 删除镜像 ###

```shell
[lighthouse@VM-12-17-centos ~]$ docker rmi d1165f221234		# 删除指定的镜像
Untagged: hello-world:latest
Untagged: hello-world@sha256:308866a43596e83578c7dfa15e27a73011bdd402185a84c5cd7f32a88b501a24
Deleted: sha256:d1165f2212346b2bab48cb01c1e39ee8ad1be46b87873d9ca7a4e434980a7726
Deleted: sha256:f22b99068db93900abe17f7f5e09ec775c2826ecfe9db961fea68293744144bd
# -f 代表强制
docker rmi -f 镜像id							# 删除指定的镜像
docker rmi -f 镜像id 镜像id 镜像id 镜像id		# 删除多个镜像
docker rmi -f $(docker images -aq)			# 删除全部镜像
```

## 容器命令 ##

### 下载一个centos镜像来测试学习

```
docker pull centos
```

### 新建容器并启动

```shell
docker run [可选参数] image 

# 参数说明
--name="name"	容器名字	mysql1	mysql2, 用来区分容器
-d				后台方式运行
-it				使用交互方式运行, 进入容器查看内容
-P 				指定容器的端口 -P 8080:8080
	-P ip:主机端口:容器端口
	-P 主机端口:容器端口	（常用）
	-P 容器端口
	容器端口
-p				随机指定端口

# 测试，启动并进入容器
[lighthouse@VM-12-17-centos ~]$ docker run -it centos /bin/bash
[root@8010a3c8725c /]# ls	# 查看容器内的centos，基本版本，很多命令都是不完善的！
bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
# 从容器中退出主机
[root@8010a3c8725c /]# exit	
[lighthouse@VM-12-17-centos /]$ ls
bin   data  etc   lib    lost+found  mnt  proc  run   srv  tmp  var
boot  dev   home  lib64  media       opt  root  sbin  sys  usr
```

### 列出所有的运行的容器

```shell
# docker ps 命令
		# 列出当前正在运行的容器
	-a	# 列出当前正在运行的容器 + 带出历史运行过的容器
	-n=?	# 显示最近创建的容器
	-q	# 只显示容器的编号

[lighthouse@VM-12-17-centos ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[lighthouse@VM-12-17-centos ~]$ docker ps -a
CONTAINER ID   IMAGE                              COMMAND       CREATED             STATUS                         PORTS     NAMES
a8c1a31d0a67   centos                             "/bin/bash"   2 minutes ago       Exited (127) 50 seconds ago              bold_greider
8010a3c8725c   centos                             "/bin/bash"   About an hour ago   Exited (0) About an hour ago             reverent_raman
e91b93dd67d4   tensorflow/tensorflow:latest-py3   "bash"        35 hours ago        Exited (0) 35 hours ago                  sad_meninsky
```

### 退出容器

```shell
exit	# 直接容器停止并退出
ctrl + P + Q	# 容器不停止退出
```

### 删除容器

```shell
docker rm 容器ID					# 删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -f
docker rm -f $(docker ps -aq)	 # 删除所有的容器
docker ps -a -q|xargs docker rm	 # 删除所有的容器 （xargs将docker ps -a -q作为参数执行docker rm）
```

### 启动和停止容器的操作

```shell
docker start 容器ID		# 启动容器
docker restart 容器ID		# 重启容器
docker stop 容器ID		# 停止当前正在运行的容器
docker kill 容器ID		# 强制停止当前容器
```

## 常用其他命令

### 后台启动容器

```shell
# 命令 docker run -d 镜像名
[lighthouse@VM-12-17-centos ~]$ docker run -d centos
50b25bef365f371678f7a5b570baf7a0db17c78ef55f4b7a36da8f353fb08088
# 问题docker ps : 发现centos停止了
[lighthouse@VM-12-17-centos ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
# 常见的坑: docker 容器使用后台运行, 就必须要有一个前台进程, docker发现没有应用, 就会自动停止
# Nginx, 容器启动后, 发现自己没有提供服务, 就会立刻停止, 就是没有程序了
```

### 查看日志

```shell
docker logs -f -t --tail 10

# 自己编写一段shell脚本
[lighthouse@VM-12-17-centos ~]$ docker run -d centos /bin/sh -c "while true;do echo jungle ni hao sh
uai;sleep 1;done"
[lighthouse@VM-12-17-centos ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
78efaf2f4c43   centos    "/bin/sh -c 'while t…"   4 seconds ago   Up 3 seconds             distracted_kepler
# 显示日志
	-tf				# 显示带时间戳的实时日志
	--tail numbers	# 要显示的日志条数
[lighthouse@VM-12-17-centos ~]$ docker logs -tf --tail 10 78efaf2f4c43
2021-03-20T12:08:23.396784354Z jungle ni hao shuai
2021-03-20T12:08:24.398066589Z jungle ni hao shuai
2021-03-20T12:08:25.399374574Z jungle ni hao shuai
2021-03-20T12:08:26.400694268Z jungle ni hao shuai
2021-03-20T12:08:27.402033270Z jungle ni hao shuai
2021-03-20T12:08:28.403442825Z jungle ni hao shuai
2021-03-20T12:08:29.404753313Z jungle ni hao shuai
2021-03-20T12:08:30.406088829Z jungle ni hao shuai
2021-03-20T12:08:31.407415166Z jungle ni hao shuai
2021-03-20T12:08:32.408697632Z jungle ni hao shuai
```

### 查看容器中的进程信息 

```shell
# ps 查看进程
# top 命令:  docker top 容器ID
[lighthouse@VM-12-17-centos ~]$ docker top 78efaf2f4c43
UID                 PID                 PPID                C                   STIME               
root                12379               12359               0                   20:04               
root                14357               12379               0                   20:13               
```

### 查看镜像的元数据

```shell
# 命令 : docker inspect 容器ID

# 测试
[lighthouse@VM-12-17-centos ~]$ docker inspect 78efaf2f4c43
[
    {
        "Id": "78efaf2f4c43db3c820c8f605329d780645f80194657d5ced11551b67b2b7b04",
        "Created": "2021-03-20T12:04:01.724433293Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo jungle ni hao shuai;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 12379,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-03-20T12:04:02.027828512Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/78efaf2f4c43db3c820c8f605329d780645f80194657d5ced11551b67b2b7b04/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/78efaf2f4c43db3c820c8f605329d780645f80194657d5ced11551b67b2b7b04/hostname",
        "HostsPath": "/var/lib/docker/containers/78efaf2f4c43db3c820c8f605329d780645f80194657d5ced11551b67b2b7b04/hosts",
        "LogPath": "/var/lib/docker/containers/78efaf2f4c43db3c820c8f605329d780645f80194657d5ced11551b67b2b7b04/78efaf2f4c43db3c820c8f605329d780645f80194657d5ced11551b67b2b7b04-json.log",
        "Name": "/distracted_kepler",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/507ab8f8cd878d1670120949a21eac56b1e2f174abb87fdc42e3a8ab06bbcb62-init/diff:/var/lib/docker/overlay2/e595330d34e5a2d0617df5454aece9979b6879b9863e6b1b8e319a567f41176f/diff",
                "MergedDir": "/var/lib/docker/overlay2/507ab8f8cd878d1670120949a21eac56b1e2f174abb87fdc42e3a8ab06bbcb62/merged",
                "UpperDir": "/var/lib/docker/overlay2/507ab8f8cd878d1670120949a21eac56b1e2f174abb87fdc42e3a8ab06bbcb62/diff",
                "WorkDir": "/var/lib/docker/overlay2/507ab8f8cd878d1670120949a21eac56b1e2f174abb87fdc42e3a8ab06bbcb62/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "78efaf2f4c43",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo jungle ni hao shuai;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "381ff0caaea5162c30478b15308a93a9db13a778ddc140aa593d7892b5a6d23b",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/381ff0caaea5",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "5e28e4fffd39a33fd874b0e6183e0a9ca181e528811fd68bdc8ac8cfbc9257aa",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "5778f0dc7937cb1ddefe5d199c42280be41b4e87661efd20d833a5a996b54317",
                    "EndpointID": "5e28e4fffd39a33fd874b0e6183e0a9ca181e528811fd68bdc8ac8cfbc9257aa",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

### 进入当前正在运行的容器

```shell
# 我们通常容器都是使用后台方式运行的, 需要进入容器, 修改一些配置

# 命令 docker exec -it 容器ID bashSell

# 测试
[lighthouse@VM-12-17-centos ~]$ docker exec -it 78efaf2f4c43 /bin/bash
[root@78efaf2f4c43 /]# ls
bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@78efaf2f4c43 /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 12:04 ?        00:00:00 /bin/sh -c while true;do echo jungle ni hao shuai;sl
root      1492     0  0 12:28 pts/0    00:00:00 /bin/bash
root      1524     1  0 12:29 ?        00:00:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /u
root      1525  1492  0 12:29 pts/0    00:00:00 ps -ef

# 方式二
docker attach 容器ID
# 测试
[lighthouse@VM-12-17-centos ~]$ docker attach 78efaf2f4c43
正在执行的代码...
jungle ni hao shuai
jungle ni hao shuai
jungle ni hao shuai
jungle ni hao shuai
jungle ni hao shuai
jungle ni hao shuai
jungle ni hao shuai
jungle ni hao shuai

# docker exec		# 进入容器后开启一个新的终端, 可以在里面操作(常用)
# docker attach		# 进入容器正在执行的终端, 不会启动新的进程!
```

### 从容器内拷贝文件到主机上

```shell
docker cp 容器ID:容器内路径 目的主机路径

# 查看当前主机目录下
[lighthouse@VM-12-17-centos ~]$ ls
[lighthouse@VM-12-17-centos ~]$ cd ..
[lighthouse@VM-12-17-centos home]$ ls
lighthouse
[lighthouse@VM-12-17-centos ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
495db60bc5b7   centos    "/bin/bash"   36 minutes ago   Up 24 minutes             quirky_franklin

# 进入docker容器内部
[lighthouse@VM-12-17-centos home]$ docker attach 495db60bc5b7
[root@495db60bc5b7 /]# cd /home
[root@495db60bc5b7 home]# ls

# 在容器内新建一个文件
[root@495db60bc5b7 /]# touch jungle.java
[root@495db60bc5b7 home]# exit
exit
[root@495db60bc5b7 home]# ls
jungle.java

[lighthouse@VM-12-17-centos home]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[lighthouse@VM-12-17-centos home]$ docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                      PORTS     NAMES
495db60bc5b7   centos    "/bin/bash"   38 minutes ago   Exited (0) 13 seconds ago            quirky_franklin

# 将文件拷贝出来到主机上
[lighthouse@VM-12-17-centos home]$ docker cp 495db60bc5b7:/home/jungle.java /home
open /home/jungle.java: permission denied
[lighthouse@VM-12-17-centos home]$ sudo docker cp 495db60bc5b7:/home/jungle.java /home
[lighthouse@VM-12-17-centos home]$ ls
jungle.java  lighthouse

# 拷贝是一个手动过程, 未来我们使用 -v 卷的技术, 可以实现自动同步
```

## 小结

<img src="C:\Users\LeBro\Pictures\summary.png" alt="summary" style="zoom:80%;" />



## 作业练习

### docker安装nginx

```shell
# 1、搜索镜像 search
# 2、下载镜像 pull
# 3、运行测试
[lighthouse@VM-12-17-centos ~]$ docker images
REPOSITORY              TAG          IMAGE ID       CREATED         SIZE
mysql                   latest       808391de2156   3 days ago      546MB
nginx                   latest       6084105296a9   8 days ago      133MB
centos                  latest       300e315adb2f   3 months ago    209MB
tensorflow/tensorflow   latest-py3   53187075965b   14 months ago   2.52GB

# -d 后台运行
# --name	给容器命名
# -p 宿主机端口:容器内部端口
[lighthouse@VM-12-17-centos ~]$ docker run -d --name nginx01 -p 3344:80 nginx
7ff54259c17c1996232269006d82a7a889256628ad37b3d33741c58e723eff4e
[lighthouse@VM-12-17-centos ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS              
    NAMES
7ff54259c17c   nginx     "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds   0.0.0.0:3344->80/tc
p   nginx01
[lighthouse@VM-12-17-centos ~]$ curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

# 进入容器
[lighthouse@VM-12-17-centos ~]$ docker exec -it nginx01 /bin/bash
root@7ff54259c17c:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@7ff54259c17c:/# cd /etc/nginx/
root@7ff54259c17c:/etc/nginx# ls
conf.d          koi-utf  mime.types  nginx.conf   uwsgi_params
fastcgi_params  koi-win  modules     scgi_params  win-utf
```

### docker安装tomcat

```shell
# 官方的使用
docker run -it --rm tomcat:9.0

# 之前的容器启动都是后台启动，停止了容器之后，容器还是可以查看到的
# docker run -it --rm 一般是用来测试的，用完就删除 （一般不推荐这样做）

# 下载再启动
docker pull tomcat

# 启动测试
[lighthouse@VM-12-17-centos ~]$ docker run -d -p 3355:8080 --name tomcat01 tomcat
9e2d3f52cc1ee5021f8824d71d9043ab6499e11109e27a32d02ccbbc88cc18b6

# 进入容器
[lighthouse@VM-12-17-centos ~]$ docker exec -it tomcat01 /bin/bash
root@9e2d3f52cc1e:/usr/local/tomcat#

# 发现问题：linux命令太少了，默认是最小的镜像，所有不必要的都删除掉，保证最小可运行的环境！
```

### 部署es + kibana

```shell
# es 暴露的端口很多！
# es 十分的耗内存
# es 的数据一般需要放置到安全目录！挂载
# --net somenetwork ? 网络配置

# 启动
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2

# es 十分耗内存，启动了，linux就卡住了

# 查看cpu状态（不停变化）：docker stats 
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O     BLOCK I/O      
 PIDS
a14aa21608e3   elasticsearch   0.17%     1.243GiB / 1.795GiB   69.25%    656B / 0B   322MB / 729kB  
 42
9e2d3f52cc1e   tomcat01        0.09%     67.56MiB / 1.795GiB   3.68%     656B / 0B   148MB / 0B     
 30
 
# 测试一下es是否成功了
[lighthouse@VM-12-17-centos ~]$ curl localhost:9200
{
  "name" : "a14aa21608e3",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "cgNtpJzKQFS6vFZ-uup2IA",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

# 赶紧关闭，增加内存限制
[lighthouse@VM-12-17-centos ~]$ docker stop $(docker ps -aq)
a14aa21608e3
9e2d3f52cc1e
[lighthouse@VM-12-17-centos ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 修改配置文件 -e 环境配置修改 : -e ES_JAVA_OPTS="-Xms64m -Xmx512m"
docker run -d --name elasticsearch_StrictMemory -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2

# 执行命令
[lighthouse@VM-12-17-centos ~]$ docker run -d --name elasticsearch_StrictMemory -p 9200:9200 -p 9300
:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
7da8cf0bb5a98724aa12b0f850478a94f6fda9d42c70cfdded61584848a791c4
[lighthouse@VM-12-17-centos ~]$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS
                                            NAMES
7da8cf0bb5a9   elasticsearch:7.6.2   "/usr/local/bin/dock…"   29 seconds ago   Up 27 seconds   0.0.0
.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   elasticsearch_StrictMemory

# 查看状态
[lighthouse@VM-12-17-centos ~]$ docker stats 7da8cf0bb5a9
# 进入后显示
CONTAINER ID   NAME                         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O     BL
OCK I/O       PIDS
7da8cf0bb5a9   elasticsearch_StrictMemory   0.77%     388.2MiB / 1.795GiB   21.12%    656B / 0B   11
6MB / 729kB   41
# 测试成功
[lighthouse@VM-12-17-centos ~]$ curl localhost:9200
{
  "name" : "7da8cf0bb5a9",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "eYN4svxcQ4aQMeQOP-8Lyg",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

### 使用kibana

<img src="C:\Users\LeBro\Pictures\kibana.png" alt="kibana" style="zoom:80%;" />

## 可视化



- portainer（先用这个）

- Rancher （CI/CD再用）

**什么是portainer？**

Docker图形化界面管理工具！提供一个后台面板供我们操作！

```shell
docker run -d -p 80:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer

# 测试 
自己的ip地址:你设置的端口号

# 1，设置密码创建用户
# 2，选择本地的
# 3，进入后会展示一个面板
```

展示如下：

![panel](C:\Users\LeBro\Pictures\panel.png)

可视化面板平时少用，这里主要测试玩一下！



# Docker镜像讲解

## 镜像是什么

镜像是一种轻量级，可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需要的所有内容，包括代码，运行时，库，环境变量和配置文件。

所有的应用，直接打包docker镜像，就可以直接跑起来！

如何得到镜像：

- 从远程仓库下载
- 直接拷贝一个
- 自己制作一个镜像 DockerFile

## Docker镜像加载原理

> UnionFS （联合文件系统）
>
> 我们下载的时候看到的一层一层的就是这个！

UnionFS（联合文件系统）：UnionFS 是一种分层，轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同的目录挂载到同一虚拟文件系统下（unite several directories into a single virtual filesystem）。

Union文件系统是Docker镜像的基础。

镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特点：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

> Docker镜像加载原理

docker的镜像实际上由一层层的文件系统组成，这种层级的文件系统UnionFS。

bootfs（boot file system）主要包含bootloader和kernel，bootloader主要是引导加载kernel，linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层时bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs（root file system），在bootfs之上。包含的就是典型的Linux系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。

![unionFS](C:\Users\LeBro\Pictures\unionFS.png)

**虚拟机是分钟级别，容器是秒级！**

## 分层理解

> 分层的理解

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层一层的在下载！

```shell
[lighthouse@VM-12-17-centos ~]$ docker pull redis
Using default tag: latest
latest: Pulling from library/redis
6f28985ad184: Already exists 
60e8b46025d8: Pull complete 
122fe26e50b0: Pull complete 
de3ca1eb2e20: Pull complete 
4813a7e5bd57: Pull complete 
99dd8d3a66f2: Pull complete 
Digest: sha256:e97d506be34a39fa69f45eea846080d6985c2c9ee338c0d408c7ea4347f014a5
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest

# 检查 一下层 --> Layer
[lighthouse@VM-12-17-centos ~]$ docker inspect redis
[
    {
        "Id": "sha256:a617c1c92774952d26fb87ba9a32fdc4d424fb7be02bbc84d6fefb517f3d4c6c",
        "RepoTags": [
            "redis:latest"
        ],
        "RepoDigests": [
            "redis@sha256:e97d506be34a39fa69f45eea846080d6985c2c9ee338c0d408c7ea4347f014a5"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2021-03-13T04:01:57.610005612Z",
        "Container": "a9f0ab5297860612c3f7050d86937161a03540cb5193d8bb2a0a1a2377c61265",
        "ContainerConfig": {
            "Hostname": "a9f0ab529786",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.2.1",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.2.1.tar.gz",
                "REDIS_DOWNLOAD_SHA=cd222505012cce20b25682fca931ec93bd21ae92cb4abfe742cf7b76aa907520"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"redis-server\"]"
            ],
            "Image": "sha256:e0ab32f36e0782a1420c82cee86df96571cd0dd89c8c704941418003cd81fdc4",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "19.03.12",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.2.1",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.2.1.tar.gz",
                "REDIS_DOWNLOAD_SHA=cd222505012cce20b25682fca931ec93bd21ae92cb4abfe742cf7b76aa907520"
            ],
            "Cmd": [
                "redis-server"
            ],
            "Image": "sha256:e0ab32f36e0782a1420c82cee86df96571cd0dd89c8c704941418003cd81fdc4",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 105305444,
        "VirtualSize": 105305444,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/e09ab44fd47c1744a23a10bae4688c4b859d13c7969c1c78fc3e72fe3f55d79a/diff:/var/lib/docker/overlay2/676cdd03308c83900e9e4d02a2bbdcb9b24dff44cc5968ae221311be744190b8/diff:/var/lib/docker/overlay2/8dad075029b2fc62e0c8ee3300b5319cc48deac7f1c5b4f1a4227fdc3baf514f/diff:/var/lib/docker/overlay2/541533c9c7bfb7d9c8f3f5fecc64b553e5dba78f32b1b92ed541ee05b71b3af8/diff:/var/lib/docker/overlay2/e7e5ff825cfd832e8c5ac594aa0f6a07ef69a66e79f4bccbd177b04b2a9b7f9b/diff",
                "MergedDir": "/var/lib/docker/overlay2/4ece2b0b5fe1e7f41b9bdbe59f498877f8b1faff47b5875c6b6adf2c15ad8130/merged",
                "UpperDir": "/var/lib/docker/overlay2/4ece2b0b5fe1e7f41b9bdbe59f498877f8b1faff47b5875c6b6adf2c15ad8130/diff",
                "WorkDir": "/var/lib/docker/overlay2/4ece2b0b5fe1e7f41b9bdbe59f498877f8b1faff47b5875c6b6adf2c15ad8130/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:14a1ca976738392ffa2ae4e54934ba28ab9cb756e924ad9297a4795a4adbfdf6",
                "sha256:a6b7270e6c20bae1d8c5d009171ce7db2e268c3d32e447a2ec4f33b02b256955",
                "sha256:cf2aefb51919d2cf15e3311fec4e187f968e06f168ee610d500e81757b53a3f7",
                "sha256:1d07bac5c7c7346740c618c2aa66e42d3042f1e231fc19128b90ab3839154cae",
                "sha256:9cf6f7d59322e560ceafc06f8896f3c8b0edca76b0598a4d4873850f4ee337eb",
                "sha256:e32a54e9cf7b7e9c94e5d3cd970b1f86be965a3723cc684f5013abb640e95482"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

理解：

所有的Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之上，创建新的镜像层。

> 特点

Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

## Commit镜像

```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[TAG]
```

实战测试


```shell
# 1 启动一个默认的tomcat
# 2 发现没有 webapps应用，原因是官方镜像默认 webapps目录下没有文件的！

# 3 现在拷贝一些文件进去

# 4 提交镜像，以后就使用修改过的镜像了！
[lighthouse@VM-12-17-centos ~]$ docker commit -a="jungle8884" -m="add webapps app" e26ee544e9d5 tomcat02:1.0
sha256:4ee667b1fe39bdf4ab25a2c155ba4e0db66043e5b6588921c80b6d174cdf1fa5
[lighthouse@VM-12-17-centos ~]$ docker images
REPOSITORY              TAG          IMAGE ID       CREATED          SIZE
tomcat02                1.0          4ee667b1fe39   31 seconds ago   672MB
portainer/portainer     latest       580c0e4e98b0   3 days ago       79.1MB
mysql                   latest       808391de2156   4 days ago       546MB
tomcat                  latest       08efef7ca980   8 days ago       667MB
redis                   latest       a617c1c92774   9 days ago       105MB
nginx                   latest       6084105296a9   9 days ago       133MB
centos                  latest       300e315adb2f   3 months ago     209MB
elasticsearch           7.6.2        f29a1ee41030   12 months ago    791MB
tensorflow/tensorflow   latest-py3   53187075965b   14 months ago    2.52GB
```

学习方式：实践和理论相结合！

```shell
如果你想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像，就好比一个快照。
```

**到了这里，算是入门Docker!**

------



# 容器数据卷

## 什么是容器数据卷

将应用和环境打包成一个镜像！

数据？如果数据都在容器中，那么当容器被删除后，数据就会丢失！

**需求：数据可以持久化！**

举例：MySQL删了，就删库跑路了？---> 需求：MySQL数据可以存储在本地！

容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地！

这就是卷技术！目录的挂载，将我们容器内的目录，挂载到Linux上面！

**总结：容器数据的持久化和同步操作！容器间也是可以数据共享的！**



## 使用数据卷

> 方式一：直接使用命令来挂载  -v

```shell
docker run -it -v 主机目录:容器内目录

# 测试
[lighthouse@VM-12-17-centos ~]$ docker run -it -v /home/test:/home centos /bin/bash

# 看挂载是否成功
[lighthouse@VM-12-17-centos ~]$ docker inspect 容器ID

...
"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/test",  # 主机内地址
                "Destination": "/home",  # docker容器内的地址
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
...
```

**测试文件的同步：双向绑定！**

1. 停止容器
2. 宿主机上修改文件
3. 启动容器
4. 容器内的数据依旧是同步的！

![volume](C:\Users\LeBro\Pictures\volume.png)

**以后修改只需要在本地修改即可，容器会自动同步！**



## 实战：安装MySQL

```shell
# 获取镜像
[root@VM-12-17-centos /]# docker pull mysql:5.7

# 运行容器，需要做数据挂载！
# 安装启动mysql，需要配置密码的，重点注意！
# 官方测试：docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

# 启动自己的mysql:5.7
-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置
--name 容器命名
[root@VM-12-17-centos ~]# docker run -d -p 9090:9090 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456789 --name mysql02 mysql:5.7
5c9f08aaebeacefb5efe33f862ff9c0c13a4edb67d20a71d6dcd9bfa1d4fd1b1

```





# DockerFile





# Docker网络





------

企业实战



Docker Compose



Docker Swarm



CI/CD Jenkins 流水线！