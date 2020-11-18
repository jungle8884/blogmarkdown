---
title: Java-Note13
categories:
  - Java
tags:
  - 集合
date: 2020-10-19 16:11:14
author: Jungle

---
# 集合 #

## 集合与数组的区别 ##
- 数组的长度是固定的; 集合的长度是可变的.
- 数组中存储的是同一类型的元素, 可以存储基本数据类型值; 集合存储的都是对象, 而且对象的类型可以不一致. 开发当中一般当对象多的时候, 使用集合进行存储.

## 集合框架 ##
子类共性抽取, 行成父类(接口).

- Collection接口
	1. 定义的是所有单列集合中共性的方法
	2. 所有的单列集合都可以使用共性的方法
	3. 没有带索引的方法
		- List接口
			1. 有序的集合(存储和取出元素顺序相同)
			2. 允许存储重复的元素
			3. 有索引, 可以使用普通的for循环遍历
				- Vector集合
				- ArrayList集合
				- LinkedList集合
		- Set接口
			1. 不允许存储重复元素
			2. 没有索引(不能使用普通的for循环遍历)
			3. TreeSet和HashSet集合是无序的集合(存和取的顺序可能不一致), 而LinkedHashSet集合则是有序的集合.
				- TreeSet集合
				- HashSet集合
					- LinkedHashSet集合
- 集合框架的学习方式
	- 学习顶层: 学习顶层接口/抽象类中共性的方法, 所有的子类都可以使用.
	- 使用底层: 底层不是接口就是抽象类, 无法创建对象使用, 需要使用底层的子类创建对象使用.

## Collection ##
Collection是所有单列集合的父接口, 因此在Collection中定义了单列集合(List和Set)通用的一些方法, 这些方法可以用于操作所有的单列集合.

- public boolean add(E e): 把给定的对象添加到当前集合中.
- public void clear(): 清空集合中所有的元素.
- public boolean remove(E e): 把给定的对象在当前集合中删除.
- public boolean contains(E e): 判断当前集合中是否包含给定的对象.
- public boolean isEmpty(): 判断当前集合是否为空.
- public int size(): 返回集合中元素的个数.
- public Object[] toArray(): 把集合中的元素, 存储到数组中.


**创建集合对象, 可以使用多态:**
	
	比如:	Collection<String> coll = new  ArrayList<>();
			coll.add("...")
			...
	
----------
# Iterator迭代器 #
迭代: 即Collection集合元素的通用获取方式.

-  在取元素之前先要判断集合中有没有元素, 
- 如果有, 就把这个元素取出来, 
- 继续判断, 如果还有就再取出出来.
-  一直把集合中的所有元素全部取出, 这种取出方式专业术语成为迭代.


## Iterator接口 ##
常用方法如下:

- public E next(): 返回迭代的下一个元素
- public boolean hasNext(): 如果仍有元素可以迭代, 则返回true

Collection继承自Iterable, 可以成为 "foreach" 语句的目标:

- public interface Collection<E>extends Iterable<E>
	- 要使用Iterator迭代器, 要使用Collection接口中的一个方法 --> iterator().
		- Iterator<E> iterator() 
		- 返回在此 collection 的元素上进行迭代的迭代器.
	- public interface Iterable<T>实现这个接口允许对象成为 "foreach" 语句的目标。

**迭代器的使用步骤**
1. 使用集合中的 iterator() 获取迭代器对象, 使用 Iterator 接口接收(多态)
2. 使用 Iterator 接口中的方法 hasNext 判断还有没有下一个元素.
3. 使用 Iterator 接口中的方法 next 取出集合中的下一个元素.

	例子:
	Iterator<String> it = coll.iterator();
	while(it.hasNext()) {
		System.out.println(it.next());	
	}

## 增强for循环 ##
专门用来遍历数组和集合的, 它的内部原理其实是一个Iterator迭代器, 所以在遍历的过程中, 不能对集合中的元素进行增删操作.

**格式:**
	
	for(集合/数组的数据类型 变量名 : 集合名/数组名) {
		//写操作代码
	}

----------
