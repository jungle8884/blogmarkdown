---
title: PythonNote
date:  2019-08-31 14:28:16
author: Jungle
categories:
- 其他笔记
tags:
- python
---
	### 课堂笔记
	- 数字: int, float, complex
	- 字符串可以用: r'abc', R'bcd' [文本表示]
		- 'str'	, "str", '''str'''.
		- 以字母r或R引导的表示原始字符串
		- 不讨论编码, 纯粹是字符串
		- 以Unicode在内存中表示
	- 字节串: b'hello world' 
	- 列表 [list]: [1,2,3] 方括号表示
	- 字典 [dict]: {"键":"值"}
	- 元组 [tuple]: (1,2,)	
		- 如果元组中只有一个元素的话, 后面的逗号不能重复
	- 集合 [set]: {'a', 'b', 'c'}
		- 不允许重复
	- Python中的变量并不直接存储值, 而是存储了值的内存地址或者引用.
	- id()	函数 可以查看对象的内存地址
	- is	测试两个对象是否是同一个
	- in	测试一个对象是否包含在另一个列表或集合内
	- Python属于强类型编程语言, Python解释器会根据赋值或运算来自动判断变量类型
	- Python还是一种动态类型语言, 变量的类型也是可以随时变化的.
	- isinstace() 函数 测试对象是否是某个类型的实例
	- import keyword	print(keyword.kwlist)	打印关键字
	- Python支持任意大的数字, 具体可以大到什么程度仅受内存大小的限制.
	- 由于精度的问题, 对于实数运算可能会有一定的误差, 应尽量避免在实数之间直接进行相等性测试.
		- 0.4 - 0.3 == 0.1
		- abs(0.4-0.1 -0.3) < 1e-6
	- ** 平方运算	等价于 pow()函数
	- 复数运算
		- abs(x)	计算摸
		- x.real	实部
		- x.imag	虚部
	- 连接字符串
	 	- x = 'good' + 'morning'
	 	- x = 'good' 'morning'		#仅适用于字符串常量
	 	- y = 'good' 
	 	- x = y 'morning'			#错误 不适用于字符串变量
	- type()		查看对象类型
	- encode()		对字符串进行编码  字符串-->变为字节
		- 'hello'.encode('utf-8')
	- decode()		对字符串进行解码	 字节--->变为字符串
		- _.decode('utf-8')        #_下划线代表上一次出现的内容
	- // 整除
	- -15 // 4 --> -4 					[向下取整]
	- 15.0 // 4 --> 3.0
	- pow(3, 2, 8) 						等价于(3**2) % 8
	- 1 < 2 < 3							关系运算符可以连用
	- 1 < 4 > 3
	- {1, 2, 3} < {1, 2, 3, 4}			判断是否是子集
	- 数值在内存中只有一份
	- 不支持 ++ 和 -- 运算符
	- 内置函数 [[参考链接](https://docs.python.org/zh-cn/3/library/functions.html "内置函数")]
	- map()								会根据提供的函数对指定序列做映射
		- 接受两个参数
		- 第一个为指定函数
		- 第二个为序列 [列表]							
	- filter()							过滤序列, 由符合条件的元素组成新的列表	
		- 接受两个参数
		- 第一个为判断函数
		- 第二个为序列 [列表]
	- iter()							用来生成迭代器 [只能向后移动]
		- 只有先将列表对象转换为迭代器对象后, 才能用于for循环控制
		- next()						当使用一个循环机制需要下一个项时使用
	- repr()							返回包含一个对象的可打印表示形式的字符串
	- 下面是定义一个函数的例子:
		- `def myMap(iterable, op, value):
	 	   	   if op not in '+-*/':
	         	  return 'Error operator'
	 	   	   func = lambda i : eval(repr(i) + op + repr(value))
	 	       return map(func, iterable)`
	- int()								Return an integer object constructed from a number or string x, or return 0 if no arguments are given
	- str()								返回 object 的 字符串 版本
	- reduce()							先引入 [from functools import reduce]
		- 接受两个参数
		- 以迭代的方式从左到右依次作用到一个序列上
	- reversed()						返回一个反向的 iterator
	- sorted(iterable, key=None, reverse=False)
		- iterable						可迭代对象
		- 具有两个可选参数，它们都必须指定为关键字参数
		- key 指定带有单个参数的函数，用于从 iterable 的每个元素中提取用于比较的键 (例如 key=str.lower)。 默认值为 None (直接比较元素)
		- reverse 为一个布尔值。 如果设为 True，则每个列表元素将按反向顺序比较进行排序
			- 举例:
		- L = [('b', 2), ('a', 1), ('c', 3)]
		- sorted(L, key=lambda x:x[0], reverse=True) # 按照Tuple第一个元素来判断x[0]
		- x = ['aaaa', 'bc', 'd', 'b', 'ba']
		- sorted(x, key=lambda item: (len(item), item)) # 先按长度比较, 后按字符顺序排序
	- random.shuffle()					打乱顺序
	- zip()								创建一个来自每个可迭代对象中的元素的迭代器
		- 返回一个元组的迭代器，其中的第 i 个元组包含来自每个参数序列或可迭代对象的第 i 个元素
		- 当所输入可迭代对象中最短的一个被耗尽时，迭代器将停止迭代
	- 类型转换:
		- bin()							转换成二进制
		- oct()							转换成十进制
		- hex()							转换成十六进制
		- int()							转换成整数
			- int('0x22b', 16)
			- bin(54321) # '0b1101010000110001'
			- int(bin(54321), 2)
		- float()
		- ord()							用来返回单个字符的Unicode码
		- chr()							用来返回Unicode编码对应得字符
		- str()							整体转换成字符串
		- ascii()						把对象转换为ASCII码表示形式
		- bytes()						用来生成字节串 或 把指定对象转换为特定编码得字节串
			- bytes('詹姆斯', 'gbk')		# 等价于 '詹姆斯'.encode('gbk')
			- str(_, 'gbk')				# 还原成字符串
		- isinstance(值, 类型)			判断数据类型
	- 输入输出
		- input()
		- print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
			- 将 objects 打印到 file 指定的文本流，以 sep 分隔并在末尾加上 end
			- 所有非关键字参数都会被转换为字符串，就像是执行了 str() 一样，并会被写入到流，以 sep 且在末尾加上 end 
			- file 参数必须是一个具有 write(string) 方法的对象；如果参数不存在或为 None，则将使用 sys.stdout
			- 由于要打印的参数会被转换为文本字符串，因此 print() 不能用于二进制模式的文件对象。 对于这些对象，应改用 file.write(...)。

----------

	### 读书笔记
	1. Pycharm开发快捷建
		- ctrl + F8		设置断点
		- shift + F10	运行
		- shift + F9	调试
		- F8			跳过
		- F7			进入
		- shift + F8	退出
		- ctrl + Enter	在下方新建行但不移动光标
		- Shift + Enter	在下方新建行并移到新行行首
		- Ctrl + /		注释(取消注释)选择的行
		- Ctrl + Alt + L		格式化代码(与QQ锁定热键冲突，关闭QQ的热键)
		- Ctrl + Shift + +		展开所有的代码块
		- Ctrl + Shift + -		收缩所有的代码块
		- Ctrl + Alt + I		自动缩进行
		- Alt + Enter			优化代码，提示信息实现自动导包
		- Ctrl + Shift + F		高级查找
		- Ctrl + Shift + N 		查找项目中的任何文件
		- Ctrl + N 				查找所有的类的名称
		- Ctrl + B				查看函数定义
	2. 字符串
		- title()	首字母大写
		- upper()	全部大写
		- lower()	全部小写
		- strip()	同时剔除字符串两端的空白
		- rstrip()	确保字符串末尾没有空白
		- lstrip()	剔除字符串开头的空白
		- str()		将内容转换为字符串
	3. 数字
		- ** 代表乘方
	4. 注释
		- 单行注释	#注释内容
		- 多行注释	"""注释内容""" 
	5. 列表 [由一系列按特定顺序排列的元素组成]
		- append()					在列表末尾添加元素
		- insert(索引, '插入元素')	在列表指定位置插入元素
		- del 列表名[索引]			删除指定位置的元素
		- pop()						可删除列表末尾的元素, 并且能让你接着使用它
		- pop(索引)					可以指定索引删除
		- remove('元素值')			只要知道元素值就可以删除该元素[只能删除第一个指定的值, 若出现多次, 需要循环来判断删除]
		- sort()					排序
		- sort(reverse=True)		逆序
		- sorted(列表名)				临时排序, 不影响原始的列表序列
		- reverse()					不排序, 只是反转列表元素
		- len(列表名)				获取列表的长度
	6. for item in list_of_items:
		- 缩进代码
		- 每个缩进的代码都是循环的一部分
		- 没有缩进的代码只执行一次
		- 冒号告诉python, 下一行是循环的第一行.
	7. 数值列表
		- range(起始索引, 结束索引)				生成数字集, 不包含第二个结束索引
		- list(range(起始索引, 结束索引))			将一系列数字转换为一个数字列表
		- range(起始索引, 结束索引, 指定步长)		间断生成一系列数字
		- min()								计算最小值
		- max()								计算最大值
		- sum()								计算总和
		- squares = [value**2 for value in range(1, 11)]		列表解析
		- 等价于
		- for value in range(1, 11):
		-     squares.append(value**2)
	8. 切片 [列表的一部分]
	    - [起始索引: 结束索引]					不包含第二个结束索引
	    - [起始索引:]							从起始索引到末尾
	    - [:结束索引]							从列表头开始
	    - for value in list[:3]:				遍历前三个
		-     print(value.title())
		- 复制列表
			- [:]								同时省略起始索引和结束索引
			- 举例:
				- my_foods = ['pizza', 'falafel', 'carrot cake']
				- friend_foods = my_foods[:]
				- my_foods.append('cannoli')
				- friend_foods.append('ice cream')
			- friend_foods 指向的是从 my_foods 复制出来的一个列表副本
			- 若 friend_foods = my_foods, 则 friend_foods 和 my_foods 指向同一个列表
	9. 元组 [不可变的列表]
		- 如果需要存储的一组值在程序的整个周期内都不变, 可使用元组
		- 举例:
			- dimension = (200, 50)
			- dimension[0] = 250 			会报错!	不能修改元组, 即不能给元组的元素赋值
			- dimension = (250, 100) 		但是可以给存储元组的变量赋值
	10. 代码格式
		- 缩进:	每级缩进都使用四个空格
		- 行长: 每行不超过80字符
		- 空行: 使用空行来组织程序文件
	11. if-else
		- if expression : else expression :
		- 多个else 用 elif
	12. 字典: 
		- {'键': 值}
		- Python 不关心键值对的添加顺序, 而只关心键和值之间的关联关系.
		- 新增/修改:	字典名['键'] = 值
		- 删除:	del 字典名['键']
		- 遍历: for key, value in 字典名.items(): 
			- 注: 遍历时声明两个变量用于存储键-值对中的值, 但是这两个变量可以使用任何名称.
			- for key in 字典名.keys():			# 遍历字典的所有键
			- for key in sorted(字典名.keys()):	# 按顺序遍历字典中的所有键
			- for value in 字典名.values():		# 遍历字典中的所有值
				- 若设计到重复: 
					- 则使用 for value in set(字典名.values()):
					- [set]集合: 类似于列表, 但每个元素都必须是独一无二的.
	13. 输入: input
		- message = input("提示信息")
		- print(message)
		- 若是数字类型, 则使用int()转换为数字
	14. While 条件表达式:
		- break		# 立即退出此循环
		- continue	# 返回到循环开头, 并根据条件判断决定是否继续执行 
	15. 函数: 带名字的代码块, 用于完成具体的工作.
		- def 函数名():
			- 函数体
			- ...
		- 实参: 调用函数时传递给函数的信息.
		- 形参: 在定义函数时, 函数完成其工作所需要的一项信息.
		- 模块: 扩展名为 .py 的文件
		- 导入模块时: import语句允许在当前运行的程序文件中使用模块的代码.
		- as 
			- 给函数指定别名: from 模块名 import 函数名 as 别名
			- 给模块指定别名: import 模块名 as 别名
		- 导入模块中的所有函数: from 模块名 import *
	16. 类: 模拟任何信息
		- 类中的函数称为	方法
		- 通过实例访问的变量称为	属性
		-  _init_(self, ...):
			-  self形参必不可少, 还必须位于其它参数的前面.
			-  self是一个指向实例本身的引用, 可以访问类中的属性和方法.
			-  当通过 类名() 创建实例时, 不需要传递self [自动传递的]
	
	17. 导入模块 [car 为模块名, 文件为car.py | Car, ElectricCar为类名]
		- from car import Car					# 从模块中导入一个类
		- from car import Car, ElectricCar		# 从模块中导入多个类
		- import car							# 导入整个模块
		- form 模块名 import *					# 导入模块中的所有类
	
	18. 文件
		- with open('pi_digits.txt') as file_object:
			- contents = file_object.read()
			- print(contents)
		- with 在不需要访问文件后将其关闭
		- open() 打开文件
		- read() 读取文件内容
		- 
		- 逐行读取:
			- for line in file_object:
				- print(line.rstrip())
			- 或
			- lines = file_object.readlines()	# 先将文件的各行存储在一个列表中
			- 后面可以在with代码快外使用该列表:
			- for line in lines:
				- print(line.rstrip())
		- 写入文件:
			- with open(filename, 'w') as file_object:
				- file_object.write('I love programming.')
			- 第一个参数是打开文件的名称
			- 第二个参数告诉Python, 以写入参数打开这个文件.
				- w				写入模式 [注: 若指定的文件已经存在, Python将返回文件对象前清空该文件]
				- r				读取模式
				- a				附加模式 [注: 以便将内容附加到文件末尾，而不是覆盖文件原来的内容]
				- r+			读取和写入文件的模式


​	

	19. 异常
		- try-except
		- else					依赖于 try 代码块成功执行的代码都放在 else 代码块中
	
	20. 分析文本
		- split() 				以空格为分隔符将字符串分拆成多个部分，并将这些部分都存储到一个列表中
	
	21. 存储数据
		- json.dump()			存储
			- 函数 json.dump() 接受两个实参：要存储的数据以及可用于存储数据的文件对象。
		- json.load()