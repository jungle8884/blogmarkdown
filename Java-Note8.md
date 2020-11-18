---
title: Java-Note8
categories:
  - Java
tags:
  - 匿名对象
  - ArrayList
  - String
date: 2020-09-13 19:31:32
author: Jungle

---
# Scanner #
可以实现键盘输入数据，到程序当中。

使用步骤：

1. 导包：import 包路径.类名称
	- import java.util.Scanner;
2. 创建：类名称 对象名 = new 类名称();
	- Scanner sc = new Scanner(System.in);
	- System.in：代表从键盘进行输入
3. 使用：对象名.成员方法名();
	- 获取键盘输入的一个int数字：		int num = sc.nextInt();
	- 获取键盘输入的一个字符串：String str = sc.next();


----------
# 匿名对象 #

*类名称 对象名 = new 类名称();*

只有右边的对象，没有左边的名字和赋值运算符。

**new 类名称();**

**匿名对象只能使用唯一的一次，下次再用不得不再创建一个新对象。**

----------
# Random #
1. 导包：import java.util.Random;
2. 创建：Random r = new Random();
3. 使用：
	- 获取一个随机的数字【范围是int所有范围，有正负两种】：
		
			int num = r.nextInt();
	
	- 获取一个随机的数字【参数代表了范围，左闭右开区间】：

			int num = r.nextInt(数字);
			eg: int num = r.nextInt(3);	
				[0,3)：由于是Int类型，所以实际上是 0~2
	- 若要获取 [1, n]，整体加 1 即可。
			
		    int n = 10;
			int num = r.nextInt(n) + 1;	
				

----------
# ArrayList #
数组的长度不可以发生改变，但是ArryList集合的长度是可以随意变化的。

对于ArrayList来说，有一个尖括号<E>代表泛型。

**注意：泛型只能是引用类型，不能是基本类型。**

如果内容为空，得到的是空的括号。


## 常用方法 ##
1. add
	
		public boolean add(E e)
		向集合当中添加元素，参数的类型和泛型一致。

2. get

		public E get(int index)
		从集合当中获取元素，参数是索引编号，返回值就是对应位置的元素。

3. remove

		public E remove(int index)
		从集合当中删除元素，参数是索引编号，返回值就是被删除掉的元素。

4. size

		public int size()
		获取集合的尺寸长度，返回值是集合中包含的元素个数。

**注**：对于ArryList集合来说，add添加动作一定是成功的，所以返回值可用可不用；但是对于其他集合来说，add添加动作不一定成功。


## 如何存放基本类型 ##
如果希望向集合ArrayList当中存储基本类型数据，必须使用基本类型对应的“包装类”。

	基本类型			包装类
	byte			Byte
	short			Short
	int				Integer		【特殊】
	long			Long
	float			Float
	double			Double
	char			Character	【特殊】
	boolean			Boolean

**从JDK 1.5+开始，支持自动装箱、自动拆箱。**

- 自动装箱：基本类型 ---> 包装类
- 自动拆箱：包装类 ---> 基本类型

----------
# String #
java.lang.String类代表字符串，
Java程序中的所有**字符串字面值**都作为此类的实例实现，其实就是说：程序当中所有的**双引号字符串**，**都是String类的对象**。（就算没有new，照样也是。 如： "abs"）

**字符串的特点：**

1. **字符串的内容永不可变**。【重点】
2. 正是因为字符串不可改变，所以字符串是可以共享使用的。
3. 字符串效果上相当于是char[]字符数组，但是底层原理是byte[]字节数组。

**字符串创建形式**

- 三种构造方法
	1. public String(): 创建一个空白字符，不含有任何内容。
	2. public String(char[] array): 根据字符数组的内容，来创建对应的字符串。
	3. public String(byte[] array): 根据字节数组的内容，来创建对应的字符串。

- 一种直接创建：
	- String str = "abc"; //右边直接用双引号


**字符串字面量**

*字符串常量池【在堆当中】*：程序当中直接写上的双引号字符串，就在字符串常量池中。

- 对于基本类型来说，== 是进行数值的比较。
- 对于引用类型来说，== 是进行【地址值】的比较。


*== 是进行对象的【地址值】比较，如果确实需要字符串的内容比较，可以使用以下两个方法：*

- public boolean equals(Object obj): 参数可以是任何对象，但是，只有参数是一个字符串并且内容相同的才会给true；否则返回false。
	- 注意事项：
		1. 任何对象都能用Object进行接收。
		2. equals方法具有对称性，也就是a.equals(b)和b.equals(a)效果一样。
		3. 如果比较双方一个常量一个变量，推荐把常量字符串写在前面。 如："abc".equals(str);

- public boolean equalsIgnoreCase(String str): 忽略大小写，进行内容比较。



##与获取相关的常用方法 ##
1. public int Length(): 获取字符串当中含有的字符个数，拿到字符串长度。
2. public String concat(String str): 将当前字符串和参数字符串拼接成为新的字符串并返回。
3. public char charAt(int index): 获取指定索引位置的单个字符。（索引从0开始）
4. public int indexOf(String str): 查找参数字符串在本字符串当中首次出现的索引位置，如果没有，返回-1值。


## 字符串的截取方法 ##
1. public String substring(int index): 截取从参数位置一直到字符串末尾，返回新字符串。
2. public String substring(int begin, int end): 截取从begin开始，一直到end结束，中间的字符串。 【注：**[begin,end) 左闭右开区间**】


## 与转换相关的常用方法 ##
1. public char[] toCharArray(): 将当前字符串拆分成为字符数组作为返回值。
2. public byte[] getBytes(): 获得当前字符串底层的字节数组。
3. public String replace(CharSequence oldString, CharSequence newString): 将所有出现的老字符串替换成为新的字符串, 返回替换之后的新字符串。
	- 【注： CharSequence意思就是说可以接受字符串类型。】
	- 把 CharSequence 当作 String 来使用。

## 分割字符串的方法 ##
public String[] split(String regex): 按照参数的规则，将字符串切分成若干部分。

	String[] arr1 = "aaa,bbb,ccc".split(",");
	for (int i = 0; i < arr1.length; i++){
		System.out.println(arr1[i]);
	}
	
	输出：
		aaa
		bbb
		ccc

**注：** split方法的参数其实是一个“正则表达式”，如果要按照英语**句点“.”进行切分，必须写 "\\." （加上两个反斜杠）**


----------
