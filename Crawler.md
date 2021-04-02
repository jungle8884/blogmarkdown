---
title: 什么是网络爬虫
categories:
  - 工具
tags:
  - 爬虫
  - http
date: 2019-09-30 14:35:05
author: Jungle
---
1. 什么是爬虫?
2. https 和 http:
3. url: [scheme://host:port/path/?query-string=xxx#anchor]
	- scheme: 协议, 一般为 https 或 ftp
	- host: 主机名
	- port: 端口号
	- path: 查找路径
	- query-string: 查询字符串, 多个查询用 '&' 连接
	- anchor: 锚点, 前端用于页面定位的.
4. Http请求:
	- Get: 一般情况下, 只从服务器获取数据下来, 并不会对服务器资源产生任何影响的时候会使用get请求.
	- Post: 向服务器发送数据(登陆)、上传文件等, 会对服务器资源产生影响的时候会使用post请求.
	- HEAD: 与Get方法一样, 都是向服务器发出指定资源的请求, 只不过服务器将不传回具体的内容. 使用这个方法可以在不必传输全部内容的情况下, 获取该资源的相关信息(元信息或元数据).
	- 请求头参数:
		1. User-Agent: 浏览器名称
		2. Referer: 表明当前这个请求是从哪个url过来的
		3. Cookie: http协议是无状态的, 用来保存用户信息, 辨别是否是同一个人.
	- 一个请求报文由请求行、 请求头部、 空行和请求数据4个部分组成.
		- 第一部分：请求行，用来说明请求类型,要访问的资源以及所使用的HTTP版本.
		- 第二部分：请求头部，紧接着请求行（即第一行）之后的部分，用来说明服务器要使用的附加信息
		- 第三部分：空行，请求头部后面的空行是必须的
		- 第四部分：请求数据也叫主体，可以添加任意的其他数据。
5. 常见状态码:
	- 1XX: 表示请求已被接收, 需接后续处理. 这类是临时响应, 只包含状态行和某些可选的响应头信息, 并以空行结束.
	- 2XX: 表示请求已成功被服务器接收、理解并接受.
		- 200 OK: 请求正常, 服务器正常的返回数据.
	- 3XX: 表示需要客户端采取进一步的操作才能完成请求. 通常用来重定向, 重定向目标需在本次响应中指明.
		- 301: 永久重定向.
		- 302: 临时重定向.
	- 4XX: 表示客户端可能发生错误, 妨碍服务器的处理. 该错误可能是语法错误或请求无效.
		- 400 Bad Request: 由于客户端的语法错误、 无效的请求或欺骗性路由请求, 服务器不会处理该请求.
		- 403 Forbidden: 服务器已经理解该请求, 但是拒绝执行, 将在返回的实体内描述拒绝的原因, 也可以不描述仅返回404响应.
		- 404 Not Found: 请求失败, 请求所希望得到的资源未被在服务器上发现, 但允许用户的后续请求. 被广泛运用于当服务器不想揭示为何请求被拒绝或者没有其他适合的响应可用的情况下.
	- 5XX: 表示服务器在处理请求的过程中有错误或者异常状态发生.
		- 500 Internal Server Error: 通用错误信息, 服务器遇到一个未曾预料的状况, 导致它无法完成对请求的处理, 不会给出具体错误信息.
		- 503 Service Unavaliable: 由于临时的服务器或者过载, 服务器当前无法处理请求. 这个状况是暂时的, 并且将在一段时间以后回复.
6. urllib库: 网络请求库.
	- request 常用
		- request.urlopen(url) 
		- 返回一个http.clientHTTPResponse对象, 这个对象是一个类文件句柄对象.
			- read(size) 读取所有
			- readline() 读取一行
			- readlines() 读取多行
			- getcode()	状态码
		- request.urlretrieve(url, filename)
		- parse.urlencode(params)	参数编码 
		- parse.parse_qs(qs)		参数解码
		- urlparse() 				url解析
		- urlsplit()				同上, 不过少了一个params参数
7. ProxyHandler处理器 (代理设置)
	1. 使用ProxyHandler, 传入代理构建一个handler
	2. 使用上面创建的handler构建一个opener
	3. 使用opener去发送一个请求
	4. 示例代码如下:
			from urllib import request
			
			# 使用代理
			handler = request.ProxyHandler({"https": "59.57.149.58:9999"}) # 1
			
			opener = request.build_opener(handler)	# 2
			url = "http://www.httpbin.org//ip" 
			resp = opener.open(url) # 3
			print(resp.read())
		
			输出: b'{\n  "origin": "223.104.175.125, 223.104.175.125"\n}\n'
	5. http://www.httpbin.org : 这个网站可以方便的查看http请求的一些参数
	6. 使用 urllib.request.ProxyHandler 传入一个代理, 这个代理是一个字典, 字典的 key 依赖于代理服务器能够接收的类型, 一般是 http 或者 https , 值是 ip:port .
	7. 使用上一步创建的 handler, 以及 request.bulid_opener 创建一个 opener 对象
	8. 使用上一步创建的 opener, 调用 open 函数, 发起请求.

8. Cookie的格式
	- Set-Cookie: NAME=VALUE; Expires/Max-age=DATE; Path=PATH; Domain=DOMAIN_NAME; SECURE
	- 参数意义: 
		- NAME: cookie的名字
		- VALUE: cookie的值
		- Expires: cookie的过期时间
		- Path: cookie作用的路径
		- Domain: cookie作用的域名
		- SECURE: 是否只在协议下起作用

9. 使用cookielib库和HTTPCookieProcessor模拟登录: [代码如下]

		from urllib import request
		
		csdn_url = "https://me.csdn.net/jungle8884"
		headers = {
		    'User-Agent': "Mozilla/5.0 (Windows NT 10.0; WOW64) "
		                  "AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 "
		                  "Safari/537.36",
		    'Cookie': "uuid_tt_dd=10_37481798290-1570952380922-144442; "
		              "dc_session_id=10_1570952380922.773168; "
		              "UserName=jungle8884; UserInfo=7b410f97d66c4e3da194ec7f5bebfca3; "
		              "UserToken=7b410f97d66c4e3da194ec7f5bebfca3; "
		              "UserNick=jungle8884; AU=936; UN=jungle8884; BT=1570958582094;"
		              " p_uid=U000000; notice=1; "
		              "Hm_ct_6bcd52f51e9b3dce32bec4a3997715ac=6525*1*10_37481798290-1570952380922-144442!5744*1*jungle8884!1788*1*PC_VC; "
		              "smidV2=20191014163826d7aa58a9fc4615085a36f9ec77341d0200f7406e2f7fdbd20; Hm_lvt_6bcd52f51e9b3dce32bec4a3997715ac=1572345251,1572348017,1572416761,1572436503; "
		              "announcement=%257B%2522isLogin%2522%253Atrue%252C%2522announcementUrl%2522%253A%2522https%253A%252F%252Fblogdev.blog.csdn.net%252Farticle%252Fdetails%252F102605809%2522%252C%2522announcementCount%2522%253A0%252C%2522announcementExpire%2522%253A3600000%257D;"
		              " firstDie=1; dc_tos=q06sfv; "
		              "Hm_lpvt_6bcd52f51e9b3dce32bec4a3997715ac=1572437372"
		}
		req = request.Request(url=csdn_url, headers=headers)
		resp = request.urlopen(req)
		# print(resp.read())
		with open('csdn.html', 'w', encoding='utf-8') as fp:
		    # write函数必须写入一个str的数据类型
		    # resp.read()读出来的是一个bytes数据类型
		    # bytes -> decode -> str 解码
		    # str -> encode -> bytes 编码
		    fp.write(resp.read().decode('utf-8'))


10. http.cookiejar模块:
	- 该模块主要的类有CookieJar、 FileCookieJar、 MozillaCookieJar、 LWPCookieJar. 这四个类的作用分别如下:
		1. CookieJar: 管理HTTP cookie值、 存储HTTP请求生成的cookie、 向传出的HTTP请求添加cookie的对象. 整个cookie都存储在内存中, 对CookieJar实例进行垃圾回收后cookie也将丢失.
		2. FileCookieJar(filename.delayload=None,policy=None): 从CookieJar派生而来, 用来创建FileCookieJar实例, 检索cookie信息并将cookie存储到文件中. filename是存储cookie的文件名. delayload为True时支持延迟访问文件, 即只有在需要时才读取文件或在文件中存储数据.
		3. MozillaCookieJar(filename.delayload=None,policy=None): 从FileCookieJar派生而来, 创建与Mozilla浏览器cookies.txt兼容的FileCookieJar实例.
		4. LWPCookieJar(filename.delayload=None,policy=None): 从FileCookieJar派生而来, 创建与libwww-perl标准的Set-Cookie3文件格式兼容的FileCookieJar实例.
	- 示例代码:
	
			from urllib import request, parse
			from http.cookiejar import CookieJar
			
			headers = {
			    'User-Agent': "Mozilla/5.0 (Windows NT 10.0; WOW64) "
			                  "AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 "
			                  "Safari/537.36"
			}
		
		
		​	
			def get_opener():
			    # 1. 登录
			    # 1.1 创建一个cookie_jar对象
			    cookie_jar = CookieJar()
			    # 1.2 使用cookie_jar创建一个HTTPCookieProcess对象
			    handler = request.HTTPCookieProcessor(cookie_jar)
			    # 1.3 使用上一步创建的handler创建一个opener
			    opener = request.build_opener(handler)
			    return opener
		
		
		​	
			def login_cnki(opener):
			    # 1.4 使用opener发送登录的请求(邮箱和密码)
			    data = {
			        'email': "**************@qq.com",
			        'password': "***********"
			    }
			    login_url = "https://kns.cnki.net/kns/logindigital.aspx?ParentLocation=http://www.cnki.net"
			    req = request.Request(login_url, data=parse.urlencode(data).encode('utf-8'), headers=headers)
			    opener.open(req)
		
		
		​	
			def visit_profile(opener):
			    # 2. 访问个人主页
			    # 获取个人主页的页面的时候, 不要新建一个opener
			    # 而应该使用之前那个opener, 因为之前的那个opener已经包含了
			    # 登录所需要的cookie信息
			    cnki_url = "http://my.cnki.net/"
			    req = request.Request(cnki_url, headers=headers)
			    resp = opener.open(req)
			    with open('cnki.html', 'w', encoding="utf-8") as fp:
			        fp.write(resp.read().decode('utf-8'))
		
		
		​	
			if __name__ == '__main__':
			    opener = get_opener()
			    login_cnki(opener)
			    visit_profile(opener)
11. 保存cookie到本地: 可以使用cookiejar的save方法, 并且需要指定一个文件名.
	1. 保存cookie:
			from urllib import request
			from http.cookiejar import MozillaCookieJar
			
			cookiejar = MozillaCookieJar('cookie.txt')
			handler = request.HTTPCookieProcessor(cookiejar)
			opener = request.build_opener(handler)
			
			resp = opener.open('http://httpbin.org/cookies/set?course=spider')
			
			cookiejar.save(ignore_discard=True)
	
	2. 加载cookie:	
			from urllib import request
			from http.cookiejar import MozillaCookieJar
			
			cookiejar = MozillaCookieJar('cookie.txt')
			cookiejar.load(ignore_discard=True)
			handler = request.HTTPCookieProcessor(cookiejar)
			opener = request.build_opener(handler)
			
			resp = opener.open('http://httpbin.org/cookies/set?course=spider')
			for cookie in cookiejar:
			    print(cookie)

12. requests库
	1. 安装和文档地址:
		- 安装: pip install requests
		- 文档地址: https://cn.python-requests.org/zh_CN/latest/
		- github地址: https://github.com/requests/requests
	2. 示例如下:
			import requests
		
			response = requests.get("http://www.baidu.com/")
			# print(type(response.text))
			# print(response.text)  # 查看响应内容 返回Unicode格式
			
			print(type(response.content))
			print(response.content.decode('utf-8'))   # 查看响应内容 返回字节流数据
			
			print(response.url)     # 查看完整url地址
			print(response.encoding)    # 查看响应头部字符编码
			print(response.status_code)     # 查看响应码
    3. response.text 和 response.content 的区别:
	    1. response.content: 这个是直接从网络上面抓取的数据, 没有经过任何解码, 所以是一个bytes类型. 其实在硬盘上和在网络上传输的字符串都是bytes类型.
	    2. response.text: 这个是requests库, 将response.content 进行解码的字符串. 解码需要指定一个编码方式, requests会根据自己的猜测来判断编码的方式, 所以有时候可能会出现乱码.
	4. 发送POST请求:
		1. 最基本的POST请求可以使用post方法:
			- response = requests.post("http://www.baidu.com/", data=data)
		2. 发送post请求非常简单: 直接调用 'requests.post' 方法就可以了. 如果返回的是json数据, 那么可以调用 'response.json' 来将json字符串转换为字典或者列表.
	5. requests使用代理: 在使用方法中, 传递`proxies`参数就可以了.
	6. requests处理cookie信息:
		- 如果想要在多次请求中共享cookie, 那么应该使用session. 
		- 示例代码如下:

				import requests
				
				# response = requests.get('https://www.baidu.com/')
				# print(response.cookies.get_dict())
				
				url = "http://www.renren.com/PLogin.do"
				data = {"email": "970138074@qq.com", "password": "pythonspider"}
				headers = {
				    'User-Agent': "Mozilla/5.0 (Windows NT 10.0; WOW64) "
				                  "AppleWebKit/537.36 (KHTML, like Gecko) "
				                  "Chrome/77.0.3865.90 Safari/537.36"
				}
				session = requests.Session()
				session.post(url, data=data, headers=headers)
				response = session.get("http://www.renren.com/880151247/profile")
				with open('renren.html', 'w', encoding='utf-8') as fp:
				    fp.write(response.text)

	7. requests处理不信任的ssl证书:
		- resp = requests.get(url, **vertify=false**)
		- print(resp.content.decode('utf-8'))
	
	8. XPath语法和lxml模块
		1. XPath (XML Path Language) 是一门在XML和HTML文档中查找信息的语言, 可用来在XML和HTML文档中对元素和属性进行遍历.
		2. 开发工具
			- Chrome插件 XPath Helper
			- Firefox插件 Try XPath
		3. XPath语法
			1. nodename 选取此节点的所有子节点
			2. / 		如果是在最前面, 代表从根节点选取. 否则选择某节点下的某个节点. --> 直接直接点
			3. // 		从全局节点中选择节点, 随便在哪个位置.						--> 可以选取子孙节点
			4. @ 		选取某个节点的属性.
			5. 谓词: 用来查找某个特定的节点或者包含某个指定的值的节点, 被嵌在方括号中.
				1. //body/div[1]		选取body下的第一个div
				2. //body/div[last()]	获取body下的最后一个div
				3. //body/div[position()<3]		获取body下的前两个元素
				4. //span[@class="salary"]		选取拥有class属性且等于salary的元素
				5. //span[contains(@class, "sa")]	模糊匹配, 效果同上
				6. *	通配符, 匹配任意节点.			/body/* 选取body下的所有子元素
				7. @*	匹配节点中的任意属性			//div[@*] 选取所有带有属性的div元素
				8. 运算符: 
					1. |	计算两个节点集
					2. + 	- 	*
					3. div 除法
					4. = 	!=
					5. < 	<= 	> 	>=
					6. or 	或
					7. and 	与
					8. mod 	取余
		
    9. lxml库
	    1. 使用lxml 解析
	    
		       from lxml import etree
		       
		       text = """某个网页代码"""
		
		
		   ​	
		   	def save_text():
		   	    with open("Try_lxml.html", "w", encoding="UTF-8") as fp:
		   	        html_element = etree.HTML(text)  # 解析字符串, 返回element对象
		   	        fp.write(etree.tostring(html_element, encoding="UTF-8").decode("UTF-8"))
		
		
		   ​	
		   	def parse_text():
		   	    html_element = etree.HTML(text)  # 解析字符串, 返回element对象
		   	    print(etree.tostring(html_element, encoding="UTF-8").decode("UTF-8"))
		
		
		   ​	
		   	def parse_file():
		   	    parser = etree.HTMLParser(encoding="UTF-8")     # 遇到不规范时需要指定解析器
		   	    html_element = etree.parse("Try_lxml.html", parser=parser)  # 解析文件, 返回element对象
		   	    print(etree.tostring(html_element, encoding="UTF-8").decode("UTF-8"))
		
		
   ​	
		   	if __name__ == '__main__':
		   	    save_text()
		   	    parse_file()
		
		2. lxml和xpath结合使用:
		
				from lxml import etree
				
				parser = etree.HTMLParser(encoding='UTF-8')     # 解析器
				html = etree.parse('Try_lxml.html', parser=parser)
				
				#  1. 获取所有a标签
				#  //a
				#  xpath函数返回的是一个列表
				# a_s = html.xpath("//a")
				# for a in a_s:
				#     print(a)
				
				#  2. 获取第二个a标签
				# a_2 = html.xpath("//a[1]")[0]
				# # print(a_2)
				# print(etree.tostring(a_2, encoding='utf-8').decode('utf-8'))
				
				#  3. 获取所有class等于even的a标签
				# a_3 = html.xpath("//a[@class='even']")
				# for a in a_3:
				#     print(etree.tostring(a_3, encoding='utf-8').decode('utf-8'))
				
				#  4. 获取所有a标签的href属性
				# aList = html.xpath("//a/@href")
				# for a in aList:
				#     print(a)
				
				#  5. 获取所有的信息 (纯文本)
				a_s5 = html.xpath("//a[contains(@href, 'https')]")
				for a in a_s5:
				    # 在某个标签下, 再执行xpath函数, 获取这个标签的子孙元素
				    # 那么应该在//之前加一个 . , 代表是在当前元素下获取
				    href = a.xpath(".//@href")
				    # print(href)
				    title = a.xpath(".//text()")    # 获取a标签下的文本信息
				    # print(title)
				    print(str(href) + "----" + str(title))