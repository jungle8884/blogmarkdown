---
title: git 学习记录
date: 2019-6-13 
author: Jungle
categories:
- 笔记
tags:
- git
---


1. 创建仓库： git init
2. 第二步，用命令git commit告诉Git，把文件提交到仓库：
	- git add -A 提交所有文件
	- git commit -a -m"备注信息"
3. 添加远程仓库 
	- git remote add origin git@github.com:jungle8884/algorithm
	- git push -u origin master
ps:若出现本地与远程仓库不一致们则可以使用git push -f origin master 强制推送（f代表force）

4. 替换命令
	 - alias hs='hexo clean && hexo g && hexo s'  #启动本地服务
	 - alias hd='hexo clean && hexo g && hexo d && git add . && git commit -m "update" && git push -f'  #部署博客"
5. 拉取远程服务器origin的master分支
	- git pull origin master
6. 提交单个文件
	- 提交修改内容与提交新文件是一样的两步
		 1. git add 文件名
		 2. git commit -m "add a line"		 
7. 远程代码仓库的下载
	- 先选定文件存放位置, 然后再用下面的命令.
		- git clone https://github.com/azhartalha/Traffic-Survalance-with-Computer-Vision-and-Deep-Learning.git
	- 或者
		- git clone git@github.com:azhartalha/Traffic-Survalance-with-Computer-Vision-and-Deep-Learning.git
