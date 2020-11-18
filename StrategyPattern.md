---
title: 策略模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-06-07 15:42:40
author: Jungle

---
# 引言 #

- 在软件开发中常常遇到类似这样的情况：
	- 当实现某一个功能存在多种算法或者策略，
	- 我们可以根据环境或者条件的不同选择不同的算法或者策略来完成该功能，
		- 如数据排序策略有冒泡排序、选择排序、插入排序、二叉树排序等。
- 如果使用多重条件转移语句实现（即硬编码），不但使条件语句变得很复杂，而且增加、删除或更换算法要修改原代码，不易维护，违背开闭原则。
- 如果采用策略模式就能很好解决该问题。

# 定义 #
- 它定义了算法家族, 分别封装起来, 让他们之间可以互相替换, 此模式让算法的变化, 不会影响到使用算法的客户.

# 优点 #
- 多重条件语句不易维护，而使用策略模式可以避免使用多重条件语句。
- 策略模式提供了一系列的可供重用的算法族，恰当使用继承可以把算法族的公共代码转移到父类里面，从而避免重复的代码。
- 策略模式可以提供相同行为的不同实现，客户可以根据不同时间或空间要求选择不同的。
- 策略模式提供了对开闭原则的完美支持，可以在不修改原代码的情况下，灵活增加新算法。
- 策略模式把算法的使用放到环境类中，而算法的实现移到具体策略类中，实现了二者的分离。

# 缺点 #
- 客户端必须理解所有策略算法的区别，以便适时选择恰当的算法类。
- 策略模式造成很多的策略类。

# 注 #
- 策略模式是准备一组算法，并将这组算法封装到一系列的策略类里面，作为一个抽象策略类的子类。
- 策略模式的重心不是如何实现算法，而是如何组织这些算法，从而让程序结构更加灵活，具有更好的维护性和扩展性。
- 策略模式与状态模式的结构相同，但它们的应用方法却是不同的。 
	- 状态对用户是透明的，在一次运算过程中，状态是变化的，是根据上下文内容确定的，用户不能控制状态变化。
	- 策略的选择是由用户根据处理需求事先确定的，在一次运算过程中是不变的。

# 结构 #
- 抽象策略（Strategy）类：定义了一个公共接口，各种不同的算法以不同的方式实现这个接口，环境角色使用这个接口调用不同的算法，一般使用接口或抽象类实现。
- 具体策略（Concrete Strategy）类：实现了抽象策略定义的接口，提供具体的算法实现。
- 环境（Context）类：又称上下文，持有一个策略类的引用，最终给客户端调用。


# 代码实现 #
----------
		package com.StrategyPattern.sample;
		
		//定义所有支持的算法的公共接口
		public abstract class Strategy {
		    //算法方法
		    public abstract void AlgorithmInterface();
		}

----------
		package com.StrategyPattern.sample;
		
		public class ConcreteStrategyA extends Strategy {
		    @Override
		    public void AlgorithmInterface() {
		        System.out.println("算法B实现.");
		    }
		}

----------
		package com.StrategyPattern.sample;
		
		public class ConcreteStrategyB extends Strategy {
		    @Override
		    public void AlgorithmInterface() {
		        System.out.println("算法A实现.");
		    }
		}

----------
		package com.StrategyPattern.sample;
		
		public class ConcreteStrategyC extends Strategy {
		    @Override
		    public void AlgorithmInterface() {
		        System.out.println("算法C实现.");
		    }
		}

----------
		package com.StrategyPattern.sample;
		
		//用一个ConcreteStrategy来配置, 维护一个对Strategy对象的引用.
		public class Context {
		    Strategy strategy;
		    //初始化时, 传入具体的策略对象.
		    public Context(Strategy strategy){
		        this.strategy = strategy;
		    }
		    //上下文接口
		    public void ContextInterface(){
		        //根据具体的策略对象, 调用其算法的方法.
		        strategy.AlgorithmInterface();
		    }
		}

----------
		package com.StrategyPattern;
		
		import com.StrategyPattern.sample.ConcreteStrategyA;
		import com.StrategyPattern.sample.ConcreteStrategyB;
		import com.StrategyPattern.sample.ConcreteStrategyC;
		import com.StrategyPattern.sample.Context;
		
		public class Main {
		
		    public static void main(String[] args) {
			    // sample code
		        {
		            Context context;
		            /*
		            * 由于实例化不同的策略, 所以最终在调用
		            * context.ContextInterface();时,
		            * 所获得的结果就是不尽相同.
		            * */
		            context = new Context(new ConcreteStrategyA());
		            context.ContextInterface();
		
		            context = new Context(new ConcreteStrategyB());
		            context.ContextInterface();
		
		            context = new Context(new ConcreteStrategyC());
		            context.ContextInterface();
		        }
		    }
		}

----------
**输出**

- 算法B实现.
- 算法A实现.
- 算法C实现.

----------






