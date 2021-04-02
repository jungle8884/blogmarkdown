---
title: 豆瓣排行榜小爬虫
categories:
  - 工具
tags:
  - Python
  - 爬虫
date: 2019-12-03 
author:	Jungle
---
## 目标网页: https://movie.douban.com/chart ##

**代码如下:**

	import requests
	from lxml import etree
	
	# 1. 将目标网站的信息抓取下来
	headers = {
	    'User-Agent': "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 "
	                  "Safari/537.36 ",
	    'Referer': "https://movie.douban.com/"
	}
	url = "https://movie.douban.com/chart"
	response = requests.get(url, headers=headers)
	# print(response.text)
	text = response.text


​	
​	# 2. 将抓取下来的信息按一定的规则进行提取
​	html = etree.HTML(text)
​	trs = html.xpath("//tr[@class='item']")
​	movies = []
​	for tr in trs:
​	    # print(etree.tostring(tr, encoding='UTF-8').decode('UTF-8'))
​	    a_s = tr.xpath(".//a[@class='nbg']")    # xpath 永远返回的是一个列表
​	    for a in a_s:
​	        title = a.xpath("@title")   # 电影名
​	        img = a.xpath(".//img//@src")   # 图片
​	    divs = tr.xpath(".//div[@class='pl2']")
​	    for div in divs:
​	        actors = div.xpath(".//p[@class='pl']")
​	        for actor in actors:
​	            actor = actor.xpath(".//text()")    # 演员
​	        score = div.xpath(".//span[@class='rating_nums']//text()")  # 评分
​	    movie = {
​	        'title': title,
​	        'score': score,
​	        'img': img,
​	        'actor': actor,
​	    }
​	    movies.append(movie)
​	print(movies)


**运行结果:**

		E:\Py_Code\douban_spider\venv\Scripts\python.exe E:/Py_Code/douban_spider/main.py
		[{'title': ['小丑'], 'score': ['8.8'], 'img': ['https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2567198874.jpg'], 'actor': ['2019-08-31(威尼斯电影节) / 2019-10-04(美国) / 杰昆·菲尼克斯 / 罗伯特·德尼罗 / 马克·马龙 / 莎姬·贝兹 / 谢伊·惠格姆 / 弗兰西丝·康罗伊 / 布莱恩·考伦 / 布莱恩·泰里·亨利 / 布莱特·卡伦 / 道格拉斯·霍奇斯 / 格伦·弗莱施勒 / 比尔·坎普...']}, {'title': ['好莱坞往事'], 'score': ['7.4'], 'img': ['https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2551119672.jpg'], 'actor': ['2019-05-21(戛纳电影节) / 2019-07-26(美国) / 2019(中国大陆) / 莱昂纳多·迪卡普里奥 / 布拉德·皮特 / 玛格特·罗比 / 埃米尔·赫斯基 / 玛格丽特·库里 / 蒂莫西·奥利芬特 / 茱莉亚·巴特斯 / 奥斯汀·巴特勒 / 达科塔·范宁 / 布鲁斯·邓恩 /...']}, {'title': ['爱尔兰人'], 'score': ['9.1'], 'img': ['https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2568902055.jpg'], 'actor': ['2019-09-27(纽约电影节) / 2019-11-01(美国点映) / 2019-11-27(美国网络) / 罗伯特·德尼罗 / 阿尔·帕西诺 / 乔·佩西 / 安娜·帕奎因 / 杰西·普莱蒙 / 哈威·凯特尔 / 斯蒂芬·格拉汉姆 / 鲍比·坎纳瓦尔 / 杰克·休斯顿 / 阿莱卡萨·帕拉迪诺 / 凯瑟琳·纳杜奇...']}, {'title': ['准备好了没'], 'score': ['6.8'], 'img': ['https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2559909120.jpg'], 'actor': ['2019-08-21(美国) / 萨玛拉·维文 / 安迪·麦克道威尔 / 马克·奥布莱恩 / 亚当·布罗迪 / 亨利·科泽尼 / 妮基·瓜达尼 / 梅兰妮·斯科洛凡诺 / 克里斯蒂安·布鲁恩 / 伊利斯 莱韦斯克 / 约翰·拉尔斯顿 / 达妮埃拉·巴博萨 / 伊拉娜·盖尔 /...']}, {'title': ['克劳斯：圣诞节的秘密'], 'score': ['8.6'], 'img': ['https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2570825762.jpg'], 'actor': ['2019-11-08(美国) / 拉什达·琼斯 / J·K·西蒙斯 / 琼·库萨克 / 詹森·舒瓦兹曼 / 皮尔斯·波普 / 西班牙 / 塞尔希奥·巴勃罗斯 / 96分钟 / 克劳斯：圣诞节的秘密 / 动画 / Zach Lewis / Jim Mahoney / 英语 / 西班牙语']}, {'title': ['犯罪现场'], 'score': ['6.4'], 'img': ['https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2570879785.jpg'], 'actor': ['2019-10-12(中国大陆) / 2019-10-24(中国香港) / 古天乐 / 宣萱 / 张继聪 / 谭耀文 / 姜皓文 / 李灿森 / 安志杰 / 刘心悠 / 薛凯琪 / 凌文龙 / 吴肇轩 / 颜卓灵 / 陈国邦 / 张松枝 / 蔡洁 / 徐广林 / 周祉君 / 张文杰 / 胡卓希 / 陈丽云 / 谭嘉荃 / 邹文正...']}, {'title': ['寄生虫'], 'score': ['8.7'], 'img': ['https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2561439800.jpg'], 'actor': ['2019-05-21(戛纳电影节) / 2019-05-30(韩国) / 宋康昊 / 李善均 / 赵汝贞 / 崔宇植 / 朴素丹 / 张慧珍 / 玄升玟 / 郑贤俊 / 朴叙俊 / 李静恩 / 朴明勋 / 朴根祿 / 郑益汉 / 李东勇 / 李柱亨 / 林艺恩 / 韩国 / 奉俊昊 / 132分钟 / 寄生虫 / 剧情 / 喜剧...']}, {'title': ['别告诉她'], 'score': ['7.4'], 'img': ['https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2574723117.jpg'], 'actor': ['2019-01-25(圣丹斯电影节) / 2019-07-12(美国) / 2019-11-22(中国大陆点映) / 奥卡菲娜 / 马泰 / 林晓杰 / 赵淑珍 / 卢红 / 姜永波 / 陈涵 / 水原碧衣 / 章静 / 杨学建 / 刘敬宇 / 刘锦航 / 美国 / 中国大陆 / 王子逸 / 98分钟 / 别告诉她 / 剧情 / 家庭...']}, {'title': ['唐顿庄园'], 'score': ['8.2'], 'img': ['https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2575400017.jpg'], 'actor': ['2019-09-13(英国) / 2019-12-13(中国大陆) / 休·博纳维尔 / 伊丽莎白·麦戈文 / 玛吉·史密斯 / 米歇尔·道克瑞 / 马修·古迪 / 布兰登·柯伊尔 / 琼安·弗洛加特 / 吉姆·卡特 / 佩内洛普·威尔顿 / 艾美达·斯丹顿 / 艾伦·里奇 / 劳拉·卡尔迈克尔...']}, {'title': ['小丑回魂2'], 'score': ['6.4'], 'img': ['https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2562980418.jpg'], 'actor': ['2019-09-06(美国) / 詹姆斯·麦卡沃伊 / 杰西卡·查斯坦 / 比尔·斯卡斯加德 / 比尔·哈德尔 / 索菲娅·莉莉丝 / 杰克·迪伦·格雷泽 / 菲恩·伍法德 / 杰登·马泰尔 / 特洛伊·詹姆斯 / 哈维尔·博泰特 / 杰·瑞恩 / 斯蒂芬·金 / 杰克逊·罗伯特·斯科特...']}]
		
		Process finished with exit code 0
