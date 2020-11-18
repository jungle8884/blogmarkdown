---
title: 适配器模式
categories:
  - 设计模式
tags:
  - 结构型模式
date: 2020-03-18 15:45:39
author: Jungle

---

**在应用程序中我们可能需要将两个不同接口的类之间进行通信，在不修改这两个的前提下我们可能会需要某个中间件来完成这个衔接的过程。这个中间件就是适配器。所谓适配器模式就是将一个类的接口，转换成客户期望的另一个接口。它可以让原本两个不兼容的接口能够无缝完成对接。**

**作为中间件的适配器将目标类和适配者解耦，增加了类的透明性和可复用性。**

1. 适配器模式（Adapter）的定义：将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。
	- 适配器模式分为类结构型模式和**对象结构型模式**两种，
	- 前者类之间的耦合度比后者高，且要求程序员了解现有组件库中的相关组件的内部结构，所以应用相对较少些。
2. 主要优点： 
	- 客户端通过适配器可以透明地调用目标接口。
	- 复用了现存的类，程序员不需要修改原有代码就能使用现有的适配者类。
	- 将目标类和适配者类解耦，解决了目标类和适配者类接口不一致的问题。
3. 缺点：对类适配器来说，更换适配器的实现过程比较复杂。

4. 模式的结构与实现:
	1. 怎么实现? : 类适配器模式可采用多重继承方式实现:
		- C++ 可定义一个适配器类来同时继承当前系统的业务接口和现有组件库中已经存在的组件接口
		- Java 不支持多继承，但可以定义一个适配器类来实现当前系统的业务接口，同时又继承现有组件库中已经存在的组件。
	2. 结构:
		1. 目标（Target）接口：当前系统业务所期待的接口，它可以是抽象类或接口。
		2. 适配者（Adaptee）类：它是被访问和适配的现存组件库中的组件接口。
		3. 适配器（Adapter）类：它是一个转换器，通过继承或引用适配者的对象，把适配者接口转换成目标接口，让客户按目标接口的格式访问适配者。

5. 应用场景: 
	- 以前开发的系统存在满足新系统功能需求的类，但其接口同新系统的接口不一致。
	- 使用第三方提供的组件，但组件接口定义和自己要求的接口定义不同

6. 示例代码:


----------
		package com.AdapterPattern;
		
		//目标（Target）接口：当前系统业务所期待的接口，它可以是抽象类或接口
		public interface ITarget {
		    public void request();
		}

----------
		package com.AdapterPattern;
		
		// 适配者类：它是被访问和适配的现存组件库中的组件接口。
		public class Adaptee {
		    public void specificRequset(){
		        System.out.println("适配者中的业务代码被调用!");
		    }
		}

----------
		package com.AdapterPattern;
		
		import java.lang.annotation.Target;
		
		//类适配器类
		public class ClassAdapter extends Adaptee implements ITarget {
		    public void request() {
		        specificRequset();
		    }
		}


----------
		package com.AdapterPattern;
		
		//对象适配器类
		public class ObjectAdapter implements ITarget {
		
		    private Adaptee adaptee;
		
		    public ObjectAdapter(Adaptee adaptee){
		        this.adaptee = adaptee;
		    }
		
		    public void request() {
		        adaptee.specificRequset();
		    }
		}

----------
		package com.AdapterPattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        System.out.println("类适配器模式测试：");
		        ITarget target = new ClassAdapter();
		        target.request();
		
		        System.out.println("对象适配器模式测试：");
		        Adaptee adaptee = new Adaptee();
		        target = new ObjectAdapter(adaptee);
		        target.request();
		    }
		}

----------
		输出:
			类适配器模式测试：
			适配者中的业务代码被调用!
			对象适配器模式测试：
			适配者中的业务代码被调用!

----------
