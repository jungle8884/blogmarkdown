---
title: Java-Note6
categories:
  - 后端
  - Java
tags:
  - 数组
  - 异常
date: 2020-08-14 10:34:18
author: Jungle

---
# 数组 #

是一种容器，可以同时存放多个数据值。

## 数组的特点 ##

1. 数组是一种引用数据类型
2. 数组当中的多个数据，类型必须统一
3. 数组的长度在程序运行期间不可改变

## 数组的初始化 ##

1. 动态初始化（指定长度）
	- 格式：
	
			数据类型[] 数组名称 = new 数据类型[数组长度];
	- 含义：
		- [] 		代表数组
		- new 		创建数组
		- 数组长度	保存多少个数据
2. 静态初始化（指定内容）
	- 格式：
	
			数据类型[] 数组名称 = new 数据类型[]{元素1, 元素2, ...};
	- 注： 
		- 静态初始化数组的长度可以自动推算出来
		- 初始化可以拆分成两个步骤
		- 省略格式不可以拆分
	- 省略格式：
			数据类型[] 数组名称 = {元素1, 元素2, ...};
3. 使用建议：
	1. 如果不确定数组当中的内容，用动态初始化；
	2. 已经确定了具体的内容，用静态舒适化。

## 数组的访问 ##

直接打印数组名称，得到的是数组对应的：内存地址哈希值。

### 访问格式： 数组名称[索引值]

- 索引值：就是一个int数字，代表数组当中元素的编号；
- 索引值从 **0** 开始，一直到 **数组长度-1** 为止；
- 以后说 **第几号** 元素，*不要说第几个*。

### 数组的默认值：

- 整数类型： 0
- 浮点类型： 0.0
- 字符类型：	'\u0000'
	- \u 代表unicode
	- 0000 代表十六进制数字
	- '\u0000' 不可见
- 布尔类型：	false
- 引用类型： null


----------
# 内存划分 #

1. 栈(Stack)：存放的都是方法中的局部变量。 方法的运行一定要在栈当中。
	- 局部变量：方法的参数，或者是方法{}内部的变量
	- 作用域：一旦超出作用域，立刻从栈内存当中消失。

2. 堆(Heap)：凡是new出来的东西，都在堆当中。
	- 堆内存里面的东西都有一个地址值：16进制
	- 堆内存里面的数据，都有默认值。

3. 方法区(Method Area)：存储.class相关信息，包含方法的信息。
4. 本地方法栈(Native Method Stack)：与操作系统相关
5. 寄存器(Pc Register)：与CPU相关。

----------

# 两种常见异常 #

## 数组索引越界异常 ##

```
如果访问数组元素时，索引编号并不存在，那么将会发生数组索引越界异常。

原因：索引编号写错了。
解决：修改成为存在的正确索引编号。
```



## 空指针异常 ##

```
数组必须进行new初始化才能使用其中的元素。
如果只是赋值一个null，没有进行new创建，那么将会发生空指针异常。

原因：忘了new
解决：补上new
```



----------
# 数组的操作 #

## 获取数组的长度 ##

```
格式：
数组名称.Length

这将会得到一个int数字，代表数组的长度。

数组一旦创建，程序运行期间，长度不可改变。
```



## 遍历数组 ##

```
for循环，并采用 数组名称.Length 控制次数。
```



## 数组元素的反转 ##

要求不能使用新数组，就用原来的唯一一个数组。

数组元素反转，其实就是对称位置的元素交换。

- 偶数个元素：

		public class Demo01 {
		    public static void main(String[] args) {
		        //数组元素反转
		        int[] arryA = new int[] {10, 15, 6, 100, 89, 99};
		        int temp = 0;
		        for (int i = 0; i < arryA.length / 2; i++) {
		            temp = arryA[i];
		            arryA[i] = arryA[arryA.length-i-1];
		            arryA[arryA.length-i-1] = temp;
		        }
		
		        for (int i = 0; i < arryA.length; i++) {
		            System.out.println(arryA[i]);
		        }
		    }
		}
		输出：
		99
		89
		100
		6
		15
		10
		
		Process finished with exit code 0

- 奇数个元素：

		public class Demo01 {
		    public static void main(String[] args) {
		        //数组元素反转
		        int[] arryA = new int[] {10, 15, 6, 100, 89};
		        int temp = 0;
		        for (int i = 0; i < arryA.length / 2; i++) {
		            temp = arryA[i];
		            arryA[i] = arryA[arryA.length-i-1];
		            arryA[arryA.length-i-1] = temp;
		        }
		
		        for (int i = 0; i < arryA.length; i++) {
		            System.out.println(arryA[i]);
		        }
		    }
		}
		输出：
		89
		100
		6
		15
		10
		
		Process finished with exit code 0

- 其他做法：

		核心代码
		for (int min = 0, max = arryA.length-1; min < max; min++, max--) {
		        temp = arryA[min];
		        arryA[min] = arryA[max];
		        arryA[max] = temp;
		    }

----------
