---
title: 单例模式
categories:
  - 设计模式
tags:
  - 创建型模式
date: 2020-03-15 10:18:42
author: Jungle

---
# 单例模式 #
**保证一个类仅有一个实例, 并提供一个访问它的全局访问点.**

在有些系统中，为了保证数据内容的一致性，对某些类要求只能创建一个实例，这就是所谓的单例模式。
**大多数情况下，资源管理类事物都需要只创建一个实例。**

1. 单例（Singleton）模式的定义：又称为单件模式，指一个类只有一个实例，且该类能自行创建这个实例的一种模式。
2. 特点:
	- 单例类只有一个实例对象
	- 该单例对象必须由单例类自行创建，任何外部事物都不能创建该类对象
	- 单例类对外提供一个访问该单例的全局访问点
3. 实现原理: 单例模式是设计模式中最简单的模式之一。通常，普通类的构造函数是公有的，外部类可以通过“new() 构造函数”来生成多个实例。但是，如果将类的构造函数设为私有的，外部类就无法调用该构造函数，也就无法生成多个实例。这时该类自身必须定义一个静态私有实例，并向外提供一个静态的公有函数用于创建或获取该静态私有实例。

4. 应用场景:
	- 在某类只要求生成一个对象的时候，如一个班中的班长、每个人的身份证号等
	- 当对象需要被共享的场合。由于单例模式只允许创建一个对象，共享该对象可以节省内存，并加快对象访问速度。如 Web 中的配置对象、数据库的连接池等;
	- 当某类需要频繁实例化，而创建的对象又频繁被销毁的时候，如多线程的线程池、网络连接池等

5. 缺点: 如果编写的是多线程程序，则Instance 要修饰成static volatile，getInstance() 要修饰成static synchronized，否则将存在线程非安全的问题。加上这两个关键字就能保证线程安全，但是每次访问时都要同步，会影响性能，且消耗更多的资源，这是此单例模式的缺点。

# 分类 #

## 饿汉式 ##

- 饿汉式，从名字上也很好理解，就是“比较勤”，
- 实例在初始化的时候就已经建好了，不管你有没有用到，都先建好了再说。
- 好处是没有线程安全的问题，
- 坏处是浪费内存空间。

		package com.SingletonPattern;
		

		public class HungrySingleton {
		    private static final HungrySingleton instance = new HungrySingleton();
		    private HungrySingleton(){} //私有化构造方法
		    public static HungrySingleton getInstance(){
		        return  instance;
		    }
		}

		package com.SingletonPattern;

## 懒汉式 ##

- 顾名思义就是实例在用到的时候才去创建，“比较懒”，
- 用的时候才去检查有没有实例，
- 如果有则返回，没有则新建。
- 有线程安全和线程不安全两种写法，区别就是synchronized关键字。

		public class LazySingleton {
		    private static LazySingleton instance;
		    private LazySingleton(){}
		
		    public static LazySingleton getInstance(){
		        if(instance == null){
		            instance = new LazySingleton();
		        }
		        return instance;
		    }
		}

		package com.SingletonPattern;
		
## 双检锁 ##

- 双检锁，又叫双重校验锁，综合了懒汉式和饿汉式两者的优缺点整合而成。
- 看下面代码实现中，特点是在synchronized关键字内外都加了一层 if 条件判断，
- 这样既保证了线程安全，又比直接上锁提高了执行效率，还节省了内存空间。

		public class DoubleLockSingleton {
		    /*
		    * 由于 DoubleLockSingleton instance 对象的创建在JVM中可能会进行重排序，
		    * 在多线程访问下存在风险，使用 volatile 修饰 instance 实例变量有效，解决该问题。
		    */
		    private volatile static DoubleLockSingleton instance; //保证 instance 在所有线程中同步
		
		    private  DoubleLockSingleton(){}  //private 避免类在外部被实例化
		    //双重检查模式，进行了两次的判断
		    public static DoubleLockSingleton getInstance(){
		        //第一次检查是避免不必要的上锁
		        if(instance == null){
		            synchronized (DoubleLockSingleton.class){
		                //第二次检查是：在给当前线程加锁后，例行检查
		                if(instance == null){
		                    instance = new DoubleLockSingleton();
		                }
		            }
		        }
		
		        return instance;
		    }
		}


7. 其余部分代码见: https://github.com/jungle8884/SingletonPattern