---
title: 工厂模式
categories:
  - 设计模式
tags:
  - 创建型模式
date: 2020-02-29 21:28:36
author: jungle

---

**源代码: https://github.com/jungle8884/FactoryMethodPattern/**

**在软件开发中能否做到软件对象的生产和使用相分离呢？能否在满足“开闭原则”的前提下，客户随意增删或改变对软件相关对象的使用呢？**

1. 工厂方法模式[Factory Method Pattern]
	- 又称工厂模式、多态工厂模式和虚拟构造器模式。定义一个创建产品对象的工厂接口，将产品对象的实际创建工作交到具体子工厂类当中，即由子工厂类来决定应该实例化（创建）哪一个类（产品对象），使系统在不修改原来代码的情况下引进新的产品.
	- 主要优点:
		- 用户只需要知道具体工厂的名称就可得到所要的产品，无须知道产品的具体创建过程；
		- 在系统增加新的产品时只需要添加具体产品类和对应的具体工厂类，无须对原工厂进行任何修改，满足开闭原则；
	- 其缺点是：
		- 每增加一个产品就要增加一个具体产品类和一个对应的具体工厂类，这增加了系统的复杂度。
	- 应用场景:
		- 客户只知道创建产品的工厂名，而不知道具体的产品名。
		- 创建对象的任务由多个具体子工厂中的某一个完成，而抽象工厂只提供创建产品的接口。
		- 客户不关心创建产品的细节，只关心产品的品牌。
	- 工厂方法模式由抽象工厂、具体工厂、抽象产品和具体产品等4个要素构成。
		1. 抽象工厂（Abstract Factory）：提供了创建产品的接口，调用者通过它访问具体工厂的工厂方法 newProduct() 来创建产品。
		2. 具体工厂（ConcreteFactory）：主要是实现抽象工厂中的抽象方法，完成具体产品的创建。
		3. 抽象产品（Product）：定义了产品的规范，描述了产品的主要特性和功能。
		4. 具体产品（ConcreteProduct）：实现了抽象产品角色所定义的接口，由具体工厂来创建，它同具体工厂之间一一对应。
		
				package com.FactoryPattern;
				
				public class Main {
				
				    public static void main(String[] args) {
					// write your code here
				        // 工厂模式
				        IVehicleFatory theFactory = new CarFactory();
				        IVehicle theVehicle = theFactory.getVehicle();
				        theVehicle.drive();
				
				        theFactory = new TractoryFactory();
				        theVehicle = theFactory.getVehicle();
				        theVehicle.drive();
				
				        theFactory = new TruckFactory();
				        theVehicle = theFactory.getVehicle();
				        theVehicle.drive();
				
				        /*
				         * 可以通过: IVehicleFatory theFactory = new CarFactory(); / new TractoryFactory(); / new TruckFactory();
				         * 来实现调用不同的方法
				         * 可以看作是一种多态, 满足多态条件:
				         * 要有继承关系
				         * 要有方法重写
				         * 要有父类引用指向子类对象
				         * */
				
				        // 简单工厂模式
				        SimpleVehicleFactory theSimpleFatory = new SimpleVehicleFactory();
				        theVehicle = theSimpleFatory.getVehicle("CAR");
				        theVehicle.drive();
				
				        theVehicle = theSimpleFatory.getVehicle("Tractor");
				        theVehicle.drive();
				
				        theVehicle = theSimpleFatory.getVehicle("Truck");
				        theVehicle.drive();
				    }
				}	
	
	- 作为一种创建类模式，在任何需要生成复杂对象的地方，都可以使用工厂方法模式
	
			- 抽象工厂:
			
					package com.FactoryPattern;
					
					public interface IVehicleFatory { // 抽象工厂
					    IVehicle getVehicle(); // 获取 IVehicle 对象
					}
			- 具体工厂: 
			- CarFactory:
					package com.FactoryPattern;
					
					public class CarFactory implements IVehicleFatory { // 具体工厂
					    @Override
					    public IVehicle getVehicle() {
					        return new Car();
					    }
					}
			- TractoryFactory:
					package com.FactoryPattern;
					
					public class TractoryFactory implements IVehicleFatory { // 具体工厂
					    @Override
					    public IVehicle getVehicle() {
					        return new Tractor();
					    }
					}
			- TruckFactory:
					package com.FactoryPattern;
					
					public class TruckFactory implements IVehicleFatory { // 具体工厂
					    @Override
					    public IVehicle getVehicle() {
					        return new Truck();
					    }
					}
			
			- 抽象产品:

					package com.FactoryPattern;
					
					public interface IVehicle { // 抽象产品
					    void drive();
					}
			
			- 具体产品:
			- Car: 
					package com.FactoryPattern;
					
					public class Car implements IVehicle { // 具体产品
					    @Override
					    public void drive() {
					        System.out.println("开小汽车");
					    }
					}

			- Tractor: 
					package com.FactoryPattern;
					
					public class Tractor implements IVehicle { // 具体产品
					    @Override
					    public void drive() {
					        System.out.println("开拖拉机");
					    }
					}
			- Truck:
					package com.FactoryPattern;
					
					public class Truck implements IVehicle { // 具体产品
					    @Override
					    public void drive() {
					        System.out.println("开卡车");
					    }
					}



	- 如果只有TRUCK、CAR和TRACTOR三种车辆，不会有其它车辆时，可以使用简单工厂模式

					package com.FactoryPattern;
					
					public class SimpleVehicleFactory { // 简单工厂模式
					    // 如果只有TRUCK、CAR和TRACTOR三种车辆，不会有其它车辆时
					    public IVehicle getVehicle(String vehicleType){
					
					        if(vehicleType.equalsIgnoreCase("CAR")){
					            return new Car();
					        }else if(vehicleType.equalsIgnoreCase("Tractor")){
					            return  new Tractor();
					        }else if(vehicleType.equalsIgnoreCase("Truck")){
					            return new Truck();
					        }
					
					        return null;
					    }
					}