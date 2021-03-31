---
title: 搭建hexo博客
date: 2019/8/10
author: Jungle
categories:
- 其他笔记
tags:
- 博客搭建
---

	1.	自己安装 Git 和 Node.js 以及注册 github 账号，然后创建Repository:yourname.github.io
	2.	生成SSH Key: ssh-keygen -t rsa -C "邮件地址@youremail.com"
	3.	一路默认，直接按Enter，然后找到.ssh文件夹里的id_rsa.pub，复制到你的github主页的Settings->SSH，
	4.	安装hexo:
		1.	cd 某个文件夹
		2.	npm install -g hexo 会创建yourname.github.io文件目录
		3.	hexo init 建立博客所需要的文件
		4.	npm install 安装依赖包
		5.	npm install hexo-deployer-git --save 确保git部署
		6.	本地查看：
			1.	hexo g 生成 g代表generate
			2.	hexo s 启动服务预览 s代表server服务器
		7.	默认主题是landscape，可以自己去网上下载一个自己喜欢的主题。
		8.	博客名字及作者信息：_config.yml
		9.	对照着看一下改：
			# Hexo Configuration
			## Docs: http://zespia.tw/hexo/docs/configure.html
			## Source: https://github.com/tommy351/hexo/
	
			# Site 这里的配置，哪项配置反映在哪里，可以参考我的博客
			title: My Blog #博客名
			subtitle: to be continued... #副标题
			description: My blog #给搜索引擎看的，对网站的描述，可以自定义
			author: Yourname #作者，在博客底部可以看到
			email: yourname@yourmail.com #你的联系邮箱
			language: zh-CN #中文。如果不填则默认英文
			
			# URL #这项暂不配置，绑定域名后，欲创建sitemap.xml需要配置该项
			## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
			url: http://yoursite.com
			root: /
			permalink: :year/:month/:day/:title/
			tag_dir: tags
			archive_dir: archives
			category_dir: categories
			
			# Writing 文章布局、写作格式的定义，不修改
			new_post_name: :title.md # File name of new posts
			default_layout: post
			auto_spacing: false # Add spaces between asian characters and western characters
			titlecase: false # Transform title into titlecase
			max_open_file: 100
			filename_case: 0
			highlight:
			enable: true
			backtick_code_block: true
			line_number: true
			tab_replace:
			
			# Category & Tag
			default_category: uncategorized
			category_map:
			tag_map:
			
			# Archives 默认值为2，这里都修改为1，相应页面就只会列出标题，而非全文
			## 2: Enable pagination
			## 1: Disable pagination
			## 0: Fully Disable
			archive: 1
			category: 1
			tag: 1
			
			# Server 不修改
			## Hexo uses Connect as a server
			## You can customize the logger format as defined in
			## http://www.senchalabs.org/connect/logger.html
			port: 4000
			logger: false
			logger_format:
			
			# Date / Time format 日期格式，可以修改成自己喜欢的格式
			## Hexo uses Moment.js to parse and display date
			## You can customize the date format as defined in
			## http://momentjs.com/docs/#/displaying/format/
			date_format: YYYY-M-D
			time_format: H:mm:ss
			
			# Pagination 每页显示文章数，可以自定义，贴主设置的是10
			## Set per_page to 0 to disable pagination
			per_page: 10
			pagination_dir: page
			
			# Disqus Disqus插件，我们会替换成“多说”，不修改
			disqus_shortname:
			
			# Extensions 这里配置站点所用主题和插件，暂时默认
			## Plugins: https://github.com/tommy351/hexo/wiki/Plugins
			## Themes: https://github.com/tommy351/hexo/wiki/Themes
			theme: landscape
			exclude_generator:
			plugins:
			- hexo-generator-feed
			- hexo-generator-sitemap
			
			# Deployment 站点部署到github要配置
			## Docs: http://zespia.tw/hexo/docs/deploy.html
			deploy:
			type: git
			repository:git@github.com:username/username.github.io.git
			branch: master
		10. 每次修改都要hexo g 生产一下
			1. hexo g
			2. hexo d
		11. 浏览器输入：username.github.io 即可查看自己的博客。
		12. 个人博客：https://jungle8884.github.io