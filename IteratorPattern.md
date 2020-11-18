---
title: 迭代器模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-05-13 21:37:35
author: Jungle

---
# 引言 #
	
	迭代器模式是针对集合对象而生的，对于集合对象而言，肯定会涉及到对集合的添加和删除操作，同时也肯定支持遍历集合元素的操作，
	我们此时可以把遍历操作放在集合对象中，但这样的话，集合对象既承担太多的责任了，
	面向对象设计原则中有一条就是单一职责原则，所以我们要尽可能地分离这些职责，用不同的类承担不同的责任，迭代器模式就是用迭代器类来承担遍历集合的职责。
	而且这种方式不利于程序的扩展，如果要更换遍历方法就必须修改程序源代码，这违背了 “开闭原则”
	既然将遍历方法封装在聚合类（集合对象中）中不可取，那么聚合类中不提供遍历方法，将遍历方法由用户自己实现是否可行呢？
	答案是同样不可取，因为这种方式会存在两个缺点：
		1、暴露了聚合类的内部表示，使其数据不安全；
		2、增加了客户的负担。
	“迭代器模式”能较好地克服以上缺点，它在客户访问类与聚合类之间插入一个迭代器，
	这分离了聚合对象与其遍历行为，对客户也隐藏了其内部细节，
	且满足“单一职责原则”和“开闭原则”，如 Java 中的 Collection、List、Set、Map 等都包含了迭代器。

# 定义 #
	迭代器模式提供了一种方法顺序访问一个聚合对象中的各个元素，而又无需暴露该对象的内部实现，
	这样既可以做到不暴露集合的内部结构，又可让外部代码透明地访问集合内部的数据。

# 优缺点 #

## 优点 ##
- 使得访问一个聚合对象的内容而无需暴露它的内部表示，即迭代抽象。
- 为遍历不同的集合结构提供了一个统一的接口，从而支持同样的算法在不同的集合结构上进行操作

## 缺点 ##
- 增加了类的个数，这在一定程度上增加了系统的复杂性。
- 迭代器模式是通过将聚合对象的遍历行为分离出来，抽象成迭代器类来实现的，其目的是在不暴露聚合对象的内部结构的情况下，让外部代码透明地访问聚合的内部数据。

# 使用场景 #
- 访问一个集合对象的内容而无需暴露它的内部表示
- 为遍历不同的集合结构提供一个统一的接口
- 就目前来说，充分利用相关语言提供的工具即可，如 Java 的 Collection、List、Set、Map等。

# 实现代码 #

----------
		package com.IteratorPattern;
		
		interface Iterator {
		    //得到开始对象
		    public abstract Object First();
		    //得到下一对象
		    public abstract Object Next();
		    //判断是否到结尾
		    public abstract boolean IsDone();
		    //当前对象
		    public abstract Object CurrentItem();
		}

----------
		package com.IteratorPattern;
		
		import java.util.ArrayList;
		import java.util.List;
		
		//具体迭代器类
		public class ConcreteIterator implements Iterator {
		    //定义了一个具体聚集对象
		    private List list = new ArrayList();
		    private int current = 0;
		
		    public ConcreteIterator(List list){
		        this.list = list;
		    }
		
		    @Override
		    public Object First() {
		        return list.get(0);
		    }
		
		    @Override
		    public Object Next() {
		        Object ret = null;
		        current++;
		        if(current < list.size()){
		            ret = list.get(current);
		        }
		        return ret;
		    }
		
		    @Override
		    public boolean IsDone() {
		        if(current == list.size()){
		            return true;
		        }
		        return false;
		    }
		
		    @Override
		    public Object CurrentItem() {
		        return list.get(current);
		    }
		}

----------
		package com.IteratorPattern;
		
		//聚集抽象类
		interface Aggregate {
		    public void add(Object obj);
		    public void remove(Object obj);
		    //创建迭代器
		    public abstract Iterator CreateIterator();
		}

----------
		package com.IteratorPattern;
		
		import java.util.ArrayList;
		import java.util.List;
		
		//具体聚集类
		public class ConcreteAggregate implements Aggregate {
		    private List list = new ArrayList();
		
		    @Override
		    public void add(Object obj) {
		        list.add(obj);
		    }
		
		    @Override
		    public void remove(Object obj) {
		        list.remove(obj);
		    }
		
		    @Override
		    public Iterator CreateIterator() {
		        return new ConcreteIterator(list);
		    }
		}

----------
		package com.IteratorPattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        Aggregate aggregate = new ConcreteAggregate();//公交车
		
		        /*新来的乘客*/
		        aggregate.add("大鸟");
		        aggregate.add("小蔡");
		        aggregate.add("行李");
		        aggregate.add("老外");
		        aggregate.add("公交内部员工");
		        aggregate.add("小偷");
		
		        //售票员
		        Iterator i = aggregate.CreateIterator();
		
		        Object item = i.First();
		        while(!i.IsDone()){
		            System.out.println(i.CurrentItem() + "请买车票!");
		            i.Next();
		        }
		    }
		}

----------
**输出:**

		大鸟请买车票!
		小蔡请买车票!
		行李请买车票!
		老外请买车票!
		公交内部员工请买车票!
		小偷请买车票!
		
		Process finished with exit code 0

----------
**代码参考自:**

- https://blog.csdn.net/zhengzhb/article/details/7610745
- 大话设计模式

----------


