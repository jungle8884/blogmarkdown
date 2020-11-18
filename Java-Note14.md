---
title: Java-Note14
categories:
  - Java
tags:
  - 泛型
date: 2020-11-02 15:36:23
author: Jungle

---
# 泛型 #
是一种未知的数据类型, 当我们不知道使用什么数据类型的时候, 可以使用泛型类型.

## 定义和使用含有泛型的类 ##
创建对象的时候确定泛型的数据类型

- 格式:

	修饰符 class 类名<代表泛型的变量> {}


## 定义一个含有泛型的方法 ##
泛型定义在方法的修饰符和返回值类型之间

- 格式:

	修饰符 <泛型> 返回值类型 方法名(参数列表(使用泛型)) {
		方法体;
	}

- 调用含有泛型的方法, 传递什么类型, 泛型就是什么类型.


## 定义含有泛型的接口 ##

- 格式:

	修饰符 interface 接口名<代表泛型的变量> {}

- 使用方式:
	1. 定义接口的实现类, 实现接口, 指定接口的泛型.

		public interface Iterator<E> {
			E next();
		}

		Scanner类实现了Iterator接口, 并指定接口的泛型类型为String, 所以重写的next方法泛型默认就是String.
		
		public final class Scanner implements Iterator<String> {
			public String next() {}
		}

	2. 接口使用什么泛型, 实现类就使用什么泛型, 类跟着接口走.

		public interface List<E> {
			boolean add(E e);
			E get(int index);
		}

		public class ArrayList<E> implements List<E> {
			public boolean add(E e) {}
			public E get(int index) {}
		}

## 泛型通配符 ##
- 不知道使用什么类型来接收的时候, 此时可以使用 ?, ? 表示未知通配符.
- 此时只能接收数据, 不能往该集合中存储数据.

		package demo;
		
		import java.util.ArrayList;
		import java.util.Iterator;
		
		public class demo01Generic {
		    public static void main(String[] args) {
		        ArrayList<Integer> list01 = new ArrayList<>();
		        list01.add(1);
		        list01.add(2);
		
		        ArrayList<String> list02 = new ArrayList<>();
		        list02.add("a");
		        list02.add("b");
		
		        printArray(list01);
		        printArray(list02);
		    }
		
		    /*
		    * 定义一个方法, 能遍历所有类型的ArrayList集合,
		    * 这时候不知道ArrayList集合使用什么数据类型,
		    * 可以使用泛型通配符 ? 来接收数据类型.
		    * 注意: 泛型没有继承概念, 不能使用Object.
		    * */
		    public static void printArray(ArrayList<?> list){
		        Iterator<?> it = list.iterator();
		        while (it.hasNext()){
		            // it.next() 方法, 取出的元素是 Object, 可以接收任意的数据类型.
		            Object o = it.next();
		            System.out.println(o);
		        }
		    }
		}
		
		1
		2
		a
		b
		
		Process finished with exit code 0


- 泛型的上限限定: 
	- ? extends E 
	- 代表使用的泛型只能是E类型的子类/本身
- 泛型的下限限定: 
	- ? super E
	- 代表使用的泛型只能是E类型的父类/本身

- 类与类之间的继承关系
	- Integer extends Number extends Object
	- String extends Object

			package demo;
			
			import java.util.ArrayList;
			import java.util.Collection;
			
			public class demo02Generic {
			    public static void main(String[] args) {
			        Collection<Integer> i_list = new ArrayList<Integer>();
			        Collection<String> s_list = new ArrayList<String>();
			        Collection<Number> n_list = new ArrayList<Number>();
			        Collection<Object> o_list = new ArrayList<Object>();
			
			        getElementExtend(i_list);
			        getElementExtend(s_list); //报错
			        getElementExtend(n_list);
			        getElementExtend(o_list); //报错
			
			        getElementSuper(i_list); //报错
			        getElementSuper(s_list); //报错
			        getElementSuper(n_list);
			        getElementSuper(o_list);
			
			    }
			
			    // 泛型的上限: 此时的泛型?, 必须是Number类型或者Number类型的子类.
			    public static void getElementExtend(Collection<? extends Number> coll){}
			    // 泛型的下限: 此时的泛型?, 必须是Number类型或者Number类型的父类.
			    public static void getElementSuper(Collection<? super Number> coll){}
			}


			Error:(16, 26) java: 不兼容的类型: java.util.Collection<java.lang.Object>无法转换为java.util.Collection<? extends java.lang.Number>
			Error:(14, 26) java: 不兼容的类型: java.util.Collection<java.lang.String>无法转换为java.util.Collection<? extends java.lang.Number>
			E:\Java_Code\Example\src\demo\demo02Generic.java
			Error:(18, 25) java: 不兼容的类型: java.util.Collection<java.lang.Integer>无法转换为java.util.Collection<? super java.lang.Number>
			Error:(19, 25) java: 不兼容的类型: java.util.Collection<java.lang.String>无法转换为java.util.Collection<? super java.lang.Number>


----------


