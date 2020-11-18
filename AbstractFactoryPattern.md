---
title: 抽象工厂模式
categories:
  - 设计模式
tags:
  - 创建型模式
date: 2020-03-01 11:19:00
author: Jungle

---

**源代码: https://github.com/jungle8884/AbstractFactoryPattern**

**同种类称为同等级，也就是说：工厂方法模式只考虑生产同等级的产品，但是在现实生活中许多工厂是综合型的工厂，能生产多等级（种类） 的产品，如电脑公司既生产台式机又笔记本电脑，家具厂既生产桌子又生产椅子。
抽象工厂模式将考虑多等级产品的生产，将同一个具体工厂所生产的位于不同等级的一组产品称为一个产品族，如台式机和笔记本电脑。**

1. 抽象工厂（AbstractFactory）模式的定义: 一种为访问类提供一个创建一组相关或相互依赖对象的接口，且访问类无须指定所要产品的具体类就能得到同族的不同等级的产品的模式结构.
	- 是工厂方法模式的升级版本，工厂方法模式只生产一个等级的产品，而抽象工厂模式可生产多个等级的产品.
	- 使用抽象工厂模式一般要满足以下条件:
		1. 系统中有多个产品族，每个具体工厂创建同一族但属于不同等级结构的产品
			- 如: Apple工厂: ipad, iphone, mac 
			-   和
			-     HuaWei工厂: MatePad, Mate, MateBook
		2. 系统一次只可能消费其中某一族产品，即同族的产品一起使用
		3. 各等级产品支持相同的标准：ipad\matepad,iphone\mate,mac\macbook


2. 优点:
	- 可以在类的内部对产品族中相关联的多等级产品共同管理，而不必专门引入多个新的类来进行管理
	- 当增加一个新的产品族时不需要修改原代码，满足开闭原则

3. 缺点: **当产品族中需要增加一个新的产品时，所有的工厂类都需要进行修改**
4. 结构: 由**抽象工厂、具体工厂、抽象产品和具体产品**等 4 个要素构成，但抽象工厂中方法个数不同，抽象产品的个数也不同.
	1. 抽象工厂（Abstract Factory）：提供了创建产品的接口，它包含多个创建产品的方法，可以创建多个不同等级的产品。
	2. 具体工厂（Concrete Factory）：主要是实现抽象工厂中的多个抽象方法，完成具体产品的创建
	3. 抽象产品（Abstract Product）：定义了产品的规范，描述了产品的主要特性和功能，抽象工厂模式有多个抽象产品
	4. 具体产品（Concrete Product）：实现了抽象产品角色所定义的接口，由具体工厂来创建，它同具体工厂之间是多对一的关系
5. 应用场景: 当需要创建的对象是**一系列相互关联或相互依赖的产品族**时，如智能设备公司生产的平板电脑、手机、智能手表等.
6. 注: 
	- 当**增加一个新的产品族时**只需增加一个新的具体工厂，不需要修改原代码，**满足开闭原则**
	- 当产品族中需要**增加一个新种类的产品**时，则所有的工厂类都需要进行修改，**不满足开闭原则**
	- 另一方面，当系统中**只存在一个等级结构的产品**时，抽象工厂模式将**退化到工厂方法模式**


7. 举例说明:
	- 主程序:
	
			package com.AbstractFactoryPattern;
	
			public class Main {
			
			    public static void main(String[] args) {
				// write your code here
			        // 富士康工厂生产苹果设备
			        IElectronicsFactory electronicsFactory = new FoxConn();
			        electronicsFactory.CreatePhone();
			        electronicsFactory.CreatePad();
			        electronicsFactory.CreateComputer();
			
			        // 华为工厂生产华为设备
			        electronicsFactory = new HuaWeiFactory();
			        electronicsFactory.CreatePhone();
			        electronicsFactory.CreatePad();
			        electronicsFactory.CreateComputer();
			    }
			}

	- 抽象工厂（Abstract Factory）:

				package com.AbstractFactoryPattern;
				
				// 电子产品工厂
				public interface IElectronicsFactory {
				    // 生产手机产品
				    void CreatePhone();
				    // 生产平板电脑
				    void CreatePad();
				    // 生产电脑
				    void CreateComputer();
				}

	- 抽象产品:
		- 电脑:
				package com.AbstractFactoryPattern;
			
				// 电脑产品接口
				public interface IComputer {
				    // 定义电脑方法
				    void Writing();
				}
		
		- 平板电脑:
			
				package com.AbstractFactoryPattern;

				// 平板电脑接口
				public interface IPad {
				    // 定义平板电脑方法
				    void WatchingMovies();
				}
		- 手机:

				package com.AbstractFactoryPattern;

				// 手机接口
				public interface IPhone {
				    // 定义手机方法
				    void Reading();
				}

	- 具体工厂:
		- 富士康: 

				package com.AbstractFactoryPattern;

				class iPhone implements IPhone {
				
				    public void Reading() {
				        System.out.println("用iPhone阅读小说.");
				    }
				}
				
				class iPad implements IPad {
				
				    public void WatchingMovies() {
				        System.out.println("用iPad看电影.");
				    }
				}
				
				class MacBook implements IComputer {
				
				    public void Writing() {
				        System.out.println("用MacBook写文档.");
				    }
				}
				
				// 生产苹果设备的工厂
				public class FoxConn implements IElectronicsFactory {
				
				    public void CreatePhone() {
				        IPhone iPhone = new iPhone();
				        iPhone.Reading();
				    }
				
				    public void CreatePad() { 	
				        IPad iPad = new iPad();
				        iPad.WatchingMovies();
				    }
				
				    public void CreateComputer() {
				        IComputer macBook = new MacBook();
				        macBook.Writing();
				    }
				}

		- 华为工厂:
		- 具体产品: 

				package com.AbstractFactoryPattern;

				class Mate implements IPhone {
				
				    @Override
				    public void Reading() {
				        System.out.println("用Mate阅读.");
				    }
				}
				
				class MatePad implements IPad {
				
				    public void WatchingMovies() {
				        System.out.println("用MateBook看电影.");
				    }
				}
				
				class MateBook implements IComputer {
				
				    public void Writing() {
				        System.out.println("用MateBook写文档.");
				    }
				}
				
				// 华为工厂
				public class HuaWeiFactory implements IElectronicsFactory {
				
				    public void CreatePhone() {
				        IPhone mate = new Mate();
				        mate.Reading();
				    }
				
				    public void CreatePad() {
				        IPad matePad = new MatePad();
				        matePad.WatchingMovies();
				    }
				
				    public void CreateComputer() {
				        IComputer mateBook = new MateBook();
				        mateBook.Writing();
				    }
				}


	
	
		