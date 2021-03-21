---
title: Docker 入门
categories:
  - Docker
tags:
  - docker
author:
  - Jungle
date: 2021-03-19 15:27:15
---
# Docker的常用命令 #

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

![CACF41079BB0CC7C76D205248AF59908](E:\工作\QQDoc\MobileFile\CACF41079BB0CC7C76D205248AF59908.png)