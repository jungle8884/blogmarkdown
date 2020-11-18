---
title: 科学上网
date: 2019-6-13 
author: Jungle
categories:
- 日志
tags:
- ss
---
#1 搭建ss:
	使用如下命令：
	1. wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
	2. chmod +x shadowsocks-all.sh
	3. ./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
	4. 选择 shadowsocks-python

#2  一些常用命令：
	安装好以后，可执行程序位于 /etc/init.d/ 目录,名称为 shadowsocks-python，该 程序有四个选项，
	1. /etc/init.d/shadowsocks-python start #启动
	2. /etc/init.d/shadowsocks-python stop #停止
	3. /etc/init.d/shadowsocks-python restart #重启
	4. /etc/init.d/shadowsocks-python status #当前状态
	
	/etc/shadowsocks-python 目录下是服务端的配置文件 config.json，如果要对服务端配置进行更改，例如更改密码以及端口号，可以直接修改该文件。修改完文件以后，该配置并不能生效，需要自己重启 shadowsocks服务：直接使用 /etc/init.d/shadowsocks-python restart 命令重启服务。
	
	5. CentOS7的相关命令：
		firewall-cmd --list-all  //查看所有信息 
		firewall-cmd --list-port //查看开发端口	
		firewall-cmd --zone=public --remove-port=1234/tcp --permanent //删除端口 
		firewall-cmd --zone=public --remove-port=1234/udp --permanent



#3 多用户模式：
	1. vi /etc/shadowsocks-python/config.json 打开配置文件
	2. 多用户配置如下：	{
							"server":"0.0.0.0",
							"local_address":"127.0.0.1",
							"local_port":1080,
							"port_password":{
							"4278":"fuckgfw1",
							"4279":"fuckgfw2",
							"4280":"fuckgfw3",
							"4281":"fuckgfw4"
							},
							"timeout":300,
							"method":"aes-256-cfb",
							"fast_open":false
						}

	3.保存并推出：ESC : wq Enter
	4./etc/init.d/shadowsocks-python restart 重启ss服务
	5.开启端口：	示例如下
				firewall-cmd --permanent --zone=public --add-port=1234/tcp
				firewall-cmd --permanent --zone=public --add-port=1234/udp
				systemctl restart firewalld


	