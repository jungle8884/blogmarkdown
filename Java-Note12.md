---
title: Java-Note12
categories:
  - Java
tags:
  - java
  - object
  - 时间
  - 日期
date: 2020-10-14 19:28:01
author: Jungle

---
# Object类 #
类 Object 是类层次结构的根(最顶层)类, 每个类都使用 Object 作为超(父)类, 所有对象(包括数组)都实现这个类的方法.

## toString() ##
- 返回该对象的字符串表示
- 直接打印对象的名字, 其实就是调用对象的toString方法
- 反过来, 看一个对象是否重写了toString方法, 直接打印对象即可
	- 若是对象的地址值(默认), 那么没有重写
	- 否则就重写了
- 创建自定义类后, 可以直接按住 Alt + Insert 直接生成toString方法.

## equals() ##
- 指示其他某个对象是否与此对象"相等"
- 内部运用的是 == 运算符:
	- 基本数据类型: 比较的是值
	- 引用数据类型: 比较的是两个对象的地址值
- 可以重写equals方法, 比较两个对象的属性值; 否则, 默认比较的是两个对象的地址值.
	- 注意事项: 使用 equals(Object obj) 时, 隐含着一个多态
	- 多态的弊端: 无法使用子类特有的内容(属性和方法)
	- 解决: 可以使用向下转型
		- 使用时可以搭配 instanceof 关键字
		- if (obj instanceof this.getClass()) { //... }
- 可以 Alt + Insert 选择 Generate equals() and hashCode() 直接生成 equals方法.
	- 建议使用 java7 版本的生成代码

----------

# Objects类 (java.util.Objects) #
在JDK7时添加了一个Objects工具类, 提供了一些方法来操作对象, 由一些静态的实用方法组成.

- Objects.equals(对象1, 对象2)
- 对两个对象进行比较, 防止空指针异常.


# java.util.Date #
表示时间和日期的类

## System.currentTimeMillis() ##
- 类 Date 表示特定的瞬间, 精确到毫秒.
- 毫秒: 
	- 千分之一秒
	-  1000 ms = 1 s

			// 获取当前系统时间到1970年1月1日 00:00:00 经历了多少毫秒
		       System.out.println(System.currentTimeMillis());
			输出: 1602681723888

- 作用: 对时间和日期进行计算.
	- 1995-10-01 到 2020-10-01 中间一共有多少天
	- 可以把日期转换为毫秒进行计算, 
	- 计算完毕后, 再把毫秒转换为日期.
- 把日期转换为毫秒:
	- 若当前日期: 1995-01-01
	- 时间原点(0 毫秒): 1970年1月1日 00:00:00 (英国格林威治时间)
	- 就是计算当前日期到时间原点之间一共经历了多少毫秒  
- 注意: 
	- 中国属于东八区, 会把时间增加8个小时
	- 即: 1970年1月1日 00:00:00 ---> 1970年1月1日 08:00:00
- 把毫秒转换为日期:
	- 1 天 = 24 * 60 * 60 =86400 秒 = 86400 * 1000 = 86400, 000 毫秒

----------
## Date()  ##
**获取当前系统的日期和时间**

	 Date date = new Date();
	 System.out.println(date); // Wed Oct 14 21:22:03 CST 2020
	 //CST: 表示中国标准时间

## Date(Long date) ##
传递毫秒值, 把毫秒值转换为Date日期.

	Date date1 = new Date(0L);
	System.out.println(date1);
	输出:
	Thu Jan 01 08:00:00 CST 1970

## Long getTime() ##
Date的成员方法, 把日期转换为毫秒.

	long time = date.getTime(); // 1602682676856
	System.out.println(time);

----------
# DateFormat类 #
java.text.DateFormat 是日期/时间格式化子类的抽象类, 通过这个类可以在日期和文本之间转换, 也就是Date对象与String对象之间转换.
	- 格式化: 按照指定的格式, 从Date对象转换为String对象
	- 解析: 按照指定的格式, 从String对象转换为Date对象

## String format(Date date) ##
按照指定的模式, 把Date日期, 格式化为符合模式的字符串.

## Date parse(String source) ##
把符合模式的字符串, 解析为Date日期.

## java.text.SimpleDateFormat extends DateFormat ##

- 构造方法: 
	- SimpleDateFormat(String pattern) 用给定的模式和默认语言环境的日期格式符号构造 SimpleDateFormat.
	- 参数: String pattern: 传递指定的模式
	- 模式: 标志字母, 区分大小写.
		- y:	 	年
		- M: 	月
		- d: 		日
		- H: 	时
		- m: 	分
		- s: 		秒
	- 写上对应的模式, 会把模式替换为对应的日期和时间:	
	- "yyyy-MM-dd  HH:mm:ss"
	- 或
	- "yyyy年MM月dd日  HH时mm分ss秒"
	- 注意: 模式中的字母不能更改, 连接模式的符号可以改变.

- 使用DateFormat类中的format方法, 把日期解析为文本:
	
	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd  HH:mm:ss");
	Date date = new Date();
	String text = sdf.format(date);
	System.out.println(text);

- 使用DateFormat类中的parse方法, 把文本解析为日期. 
	- 该方法声明了一个ParseException解析异常
	- 如果字符串和构造方法中的模式不一样, 那么程序就会抛出异常
	- 解决方法: 要么继续声明抛出这一个异常, 要么try...catch处理这个异常

	SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日  HH时mm分ss秒");
	Date date = sdf.parse("2020年08月08日  17时01分10秒");
	System.out.println(date);

----------
# Calendar #
java.util.Calendar 日历类 是 一个抽象类, 提供了很多操作日历字段的方法(YEAR、MONTH、DAY_OF_MONTH HOUR), static Calendar getInstance() 使用默认时区和语言环境获得一个日历类的子类对象.

	Calendar c = Calendar.getInstance(); //多态
	System.out.println(c);

## 成员方法的参数 ##
- public static final int YEAR = 1:				年
- public static final int MONTH = 2:			月
- public static final int DATE = 5:				日(月中的某一天)
- public static final int DAY_OF_MONTH = 5: 	同上, 日(月中的某一天).
- public static final int HOUR = 10:			时
- public static final int MINUTE = 12:			分
- public static final int SECOND = 13:			秒

## public int get(int field) ##
返回给定日历字段的值

	Calendar c = Calendar.getInstance();
	int year = c.get(Calendar.YEAR);
	System.out.println(year);

	int month = c.get(Calendar.MONTH);
	System.out.println(month) + 1; //西方的月份0-11 东方的月份1-12

	int date = c.get(Calendar.DATE); //或Calendar.DAY_OF_MONTH
	System.out.println(date);

## public void set(int field, int value) ##
将给定的日历字段设置为给定值

	Calendar c = Calendar.getInstance();
	c.set(Calendar.YEAR, 2020);
	int year = c.get(Calendar.YEAR);
	System.out.println(year);

	c.set(Calendar.YEAR, 10);
	int month = c.get(Calendar.MONTH);	
	System.out.println(month); //西方的月份0-11 东方的月份1-12
	
	c.set(Calendar.DAY_OF_MONTH, 1);
	int date = c.get(Calendar.DAY_OF_MONTH); 
	System.out.println(date);

##public void set(int year, int month, int date)  ##
同时设置年, 月, 日.

## public abstract void add(int field, int mount) ##
根据日历的规则, 为给定的日历字段添加或减去指定的时间量.

	- field: 传递指定的日历字段
	- mount: 增加/减少的值
		- 正数: 增加
		- 负数: 减少

##  public Date getTime() ##
返回一个表示此 Calendar 时间值（从历元至现在的毫秒偏移量）的 Date 对象。

----------
# System类 #
java.lang.System类中提供了大量的静态方法, 可以获取与系统相关的信息或系统及操作.

## public static long currentTimeMillis() ##
返回以毫秒为单位的当前时间.

## public static void arraycopy(Object src, int srcPos, Object dest, int desPos, int length) ##
将数组中指定的数据拷贝到另一组数组中. 

- src 源数组。
- srcPos 源数组中的起始位置。
- dest 目标数组。
- destPos 目标数据中的起始位置。
- length 要复制的数组元素的数量。 

----------
# StringBuilder类 #
java.lang.StringBuilder 字符串缓冲区, 可以提高字符串的操作效率(看成一个长度可以变化的字符串, 如果容量超了, 会自动扩容).

- 字符串是常量；它们的值在创建之后不能更改。
- 字符串缓冲区支持可变的字符串。
- String --> StringBuilder: 可以使用 StringBuilder(String str) 构造方法.
- StringBuilder --> String: 可以使用 toString()方法 将当前StringBuilder对象转换为String对象.

## public StringBuilder append(...) ##
添加任意类型的数据的字符串形式, 并返回当前对象自身 [**无需接收返回值**].

## public String toString() ##
将当前StringBuilder对象转换为String对象.

----------
# 包装类 #
基本数据类型的数据使用起来非常方便, 但是没有对应的方法来操作这些数据, 所以可以使用一个类把这些数据类型包装起来, 然后定义一些方法用来操作基本数据类型的数据.

	基本类型 	对应的包装类
	byte		Byte
	short		Short
	int			Integer
	long		Long
	float		Float
	double		Double
	char		Character
	boolean		Boolean

## 装箱与拆箱 ##
以 int 与 Integer 为例.

- 装箱: 把基本类型的数据, 包装到包装类中 (基本类型的数据 --> 包装类).
	- public static Integer valueOf(int i):		返回一个表示指定的 int 值的 Integer 实例。
	- public static Integer valueOf(String s):	返回保存指定的 String 的值的 Integer 对象。
- 拆箱: 在包装类中取出基本类型的数据 (包装类 --> 基本类型的数据).
	- public int intValue():		以 int 类型返回该 Integer 的值。 

- Integer 重写了 toString()方法:  
	- public static String toString(int i)
	-  返回一个表示该 Integer 值的 String 对象。
	-  将该参数转换为有符号的十进制表示形式，并以字符串的形式返回它，就好像将该整数值作为参数赋予 toString(int) 方法一样。
	-  覆盖：类 Object 中的 toString
	- 返回：该对象的值（基数 10）的字符串表示形式。

## 自动装箱与自动拆箱 ##
从JDK 1.5开始, 基本数据类型的数据与包装类之间可以自动的完成相互转换.

- 自动装箱: 直接把int类型的整数赋值给包装类

	Integer in = 4; //相当于 Integer in = Integer.valueOf(4);

- 自动拆箱: in 是包装类对象, 无法直接参与运算, 可以自动转换为基本类型的数据, 再参与计算.

	in = in + 2; //相当于 in = in.intValue() + 2;  先自动拆箱, 后自动装箱.

## 字符串 --> 基本类型 ##
除了Character类之外, 其他所有包装类都具有parseXxx静态方法可以将字符串转换为对应的基本类型

- public static byte parseByte(String s): 将字符串参数转换为对应的byte基本类型.
- public static short parseShort(String s): 将字符串参数转换为对应的short基本类型.
- public static int parseInt(String s): 将字符串参数转换为对应的int基本类型.
- public static long parseLong(String s): 将字符串参数转换为对应的long基本类型.
- public static float parseFloat(String s): 将字符串参数转换为对应的float基本类型.
- public static double parseDouble(String s): 将字符串参数转换为对应的double基本类型.
- public static boolean parseBoolean(String s): 将字符串参数转换为对应的boolean基本类型.

	int num = Integer.parseInt("100");

## 基本类型 --> 字符串 ##
以 **Integer** 包装类为例:

1. 基本类型数据的值 + "" : 最简单的方式.
2. 使用包装类中的 public static String toString(int i), 返回一个表示指定整数的 String 对象.
3. 使用String类中的 static String valueOf(int i), 返回int参数的字符串表示形式.

----------