---
title: Java_Note1
categories:
  - Java
tags:
  - JVM
  - JDK
  - JRE
  - 存储单位
  - 命令提示符
date: 2020-06-14 15:39:51
author: Jungle

---
# 存储单位 #

- bit 位
- Byte 字节 (数据存储最小单位）
- 1 Byte == 8 bit

		1KB == 1024 Byte
		1MB == 1024KB
		1GB == 1024 MB
		1TB == 1024 GB
		1PB == 1024 TB
		1EB == 1024PB
		1ZB == 1024EB


----------
# 命令提示符 # 
**MS-DOS(Microsoft Disk Operating System)**

- 命令提示符(cmd) 

		启动：		win+R，输入cmd回车
		切换盘符		盘符名称：
		进入文件夹 	cd 文件夹名称
		进入多级文件夹	cd 文件夹1\文件夹2\文件夹3
		返回上一级	cd ..
		直接返回根路径	cd \
		查看当前文件夹	dir
		清屏		cls
		退出		exit


----------
# 开发环境 #
	运行已有的Java程序, 只需要安装JRE
	开发一个全新的Java程序, 必须安装JDK
	JDK(包含)-->JRE(包含)-->JVM
## JVM ##

- Java Virtual Machine: Java虚拟机
## JRE ##
- Java Runtime Environment: Java程序的运行环境
## JDK ##
- Java Development Kit: Java程序的开发工具包

----------
# 扬帆起航 #
- 入门代码:

		public class HelloWorld{
			public static void main(String[] args){ //程序执行的起点
				System.out.println("Hello World!"); 
			}	
		}

- javac HelloWorld.java //编译
- java HelloWorld //运行


----------
# 关键字 #

## 特点 ##
- 完全小写的字母
- 有特殊颜色

# 标识符 #
- 定义: 在程序中, 我们自己定义内容, 比如类的名字、 方法的名字和变量的名字等等.
- 命名规则: 可以包含26个英文字母(区分大小写)、 0-9(数字)、 $(美元符号) 和 _(下划线).
- 命名规范:
	- 类名规范: 首字母大写, 后面每个单词首字母大写(大驼峰式)
	- 变量名规范: 首字母小写, 后面每个单词首字母大写(小驼峰式)
	- 方法名规范: 同变量名.
	



  