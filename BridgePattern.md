---
title: 桥接模式
categories:
  - 设计模式
tags:
  - 结构型模式
date: 2020-03-21 10:46:45
author: Jungle

---

# 桥接模式 #
	
	
	1. 引言
		- 如果说某个系统能够从多个角度来进行分类，且每一种分类都可能会变化。如果将m个角度分类，每个分类平均有n种变化，就会得到m*n种不同种类的事物。
		- 完全以继承方式实现这种分类和变化是不可取的。我们需要做的就是将这多个角度分离出来，使得他们能独立变化，减少他们之间的耦合，这个分离过程就使用了桥接模式。
		- 所谓桥接模式就是讲抽象部分和实现部分隔离开来，使得他们能够独立变化。
		- 这种方式只需m+n种实现即可。
		- 桥接模式将继承关系转化成关联关系，封装了变化，完成了解耦，减少了系统中类的数量，也减少了代码量。


	2. 定义 
		- 将抽象与实现分离，使它们可以独立变化。
		- 它是用组合关系代替继承关系来实现，从而降低了抽象和实现这两个可变维度的耦合度。
	
	3. 优点
		- 由于抽象与实现分离，所以扩展能力强
		- 其实现细节对客户透明

	4. 缺点
		- 由于聚合关系建立在抽象层，要求开发者针对抽象化进行设计与编程，这增加了系统的理解与设计难度。

	5. 应用场景
		1. 当一个类存在两个独立变化的维度，且这两个维度都需要进行扩展时
		2. 当一个系统不希望使用继承或因为多层次继承导致系统类的个数急剧增加时
		3. 当一个系统需要在构件的抽象化角色和具体化角色之间增加更多的灵活性时


# 桥接模式的结构与实现 #

	可以将抽象化部分与实现化部分分开，取消二者的继承关系，改用组合关系
	
	1. 模式的结构:
		1. 抽象化（Abstraction）角色：定义抽象类，并包含一个对实现化对象的引用
		2. 扩展抽象化（Refined Abstraction）角色：是抽象化角色的子类，实现父类中的业务方法，并通过组合关系调用实现化角色中的业务方法
		3. 实现化（Implementor）角色：定义实现化角色的接口，供扩展抽象化角色调用
		4. 具体实现化（Concrete Implementor）角色：给出实现化角色接口的具体实现
	
	2. 具体代码:
	
----------
		package com.BridgePattern;
		
		// 抽象化（Abstraction）角色：定义抽象类，并包含一个对实现化对象的引用
		abstract class Abstraction {
		    protected Implementor imple;
		    protected Abstraction(Implementor imple){
		        this.imple = imple;
		    }
		
		    public abstract void Operation();
		}

----------
		package com.BridgePattern;

		// 具体实现化（Concrete Implementor）角色：给出实现化角色接口的具体实现
		class ConcreteImplementorA  implements Implementor{
		
		    @Override
		    public void OperationImpl() {
		        System.out.println("具体实现化(ConcreteImplementorA)角色被访问");
		    }
		}

----------
		package com.BridgePattern;
		
		// 具体实现化（Concrete Implementor）角色：给出实现化角色接口的具体实现
		class ConcreteImplementorB implements Implementor {
		    @Override
		    public void OperationImpl() {
		        System.out.println("具体实现化(ConcreteImplementorB)角色被访问");
		    }
		}


----------
		package com.BridgePattern;

		// 实现化（Implementor）角色：定义实现化角色的接口，供扩展抽象化角色调用
		interface Implementor {
		    public void OperationImpl();
		}

----------
		package com.BridgePattern;
		
		// 具体实现化（Concrete Implementor）角色：给出实现化角色接口的具体实现
		class ConcreteImplementorA  implements Implementor{
		
		    @Override
		    public void OperationImpl() {
		        System.out.println("具体实现化(ConcreteImplementorA)角色被访问");
		    }
		}

----------
		package com.BridgePattern;
		
		// 具体实现化（Concrete Implementor）角色：给出实现化角色接口的具体实现
		class ConcreteImplementorB implements Implementor {
		    @Override
		    public void OperationImpl() {
		        System.out.println("具体实现化(ConcreteImplementorB)角色被访问");
		    }
		}

----------
		package com.BridgePattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        Implementor imple = new ConcreteImplementorA();
		        Abstraction absA = new RefinedAbstractionA(imple);
		        absA.Operation();
		        imple =new ConcreteImplementorB();
		        absA = new RefinedAbstractionA(imple);
		        absA.Operation();
		
		        imple = new ConcreteImplementorA();
		        Abstraction absB = new RefinedAbstractionB(imple);
		        absB.Operation();
		        imple =new ConcreteImplementorB();
		        absB = new RefinedAbstractionA(imple);
		        absB.Operation();
		    }
		}

输出:
		扩展抽象化(RefinedAbstractionA)角色被访问
		具体实现化(ConcreteImplementorA)角色被访问
		扩展抽象化(RefinedAbstractionA)角色被访问
		具体实现化(ConcreteImplementorB)角色被访问
		扩展抽象化(RefinedAbstractionB)角色被访问
		具体实现化(ConcreteImplementorA)角色被访问
		扩展抽象化(RefinedAbstractionA)角色被访问
		具体实现化(ConcreteImplementorB)角色被访问
----------
**代码下载链接**: https://github.com/jungle8884/BridgePattern