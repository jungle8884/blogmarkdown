---
title: 模板模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-06-07 15:43:46
author: Jungle

---
# 引言 #
- 在面向对象程序设计过程中，程序员常常会遇到这种情况：
	- 设计一个系统时知道了算法所需的关键步骤，而且确定了这些步骤的执行顺序，但某些步骤的具体实现还未知，或者说某些步骤的实现与具体的环境相关。
	- 这样的例子在生活中还有很多，
		- 例如，一个人每天会起床、吃饭、做事、睡觉等，其中“做事”的内容每天可能不同。
		- 我们把这些规定了流程或格式的实例定义成模板，允许使用者根据自己的需求去更新它，
			- 例如，简历模板、论文模板、Word 中模板文件等。
- 模板方法模式将解决以上类似的问题。

# 定义 #
- 定义一个操作中的算法骨架，而将算法的一些步骤延迟到子类中，
- 使得子类可以不改变该算法结构的情况下重定义该算法的某些特定步骤.

# 优点 #
- 它封装了不变部分，扩展可变部分。它把认为是不变部分的算法封装到父类中实现，而把可变部分算法由子类继承实现，便于子类继续扩展。
- 它在父类中提取了公共的部分代码，便于代码复用。
- 部分方法是由子类实现的，因此子类可以通过扩展方式增加相应的功能，符合开闭原则。

# 缺点 #
- 对每个不同的实现都需要定义一个子类，这会导致类的个数增加，系统更加庞大，设计也更加抽象。
- 父类中的抽象方法由子类实现，子类执行的结果会影响父类的结果，这导致一种反向的控制结构，它提高了代码阅读的难度。

# 应用场景 #
- 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现；
- 各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复；
- 控制子类的扩展。

# 结构 #
- 抽象类（Abstract Class）：负责给出一个算法的轮廓和骨架。它由一个模板方法和若干个基本方法构成。这些方法的定义如下。
	- ① 模板方法TemplateMethod：定义了算法的骨架，按某种顺序调用其包含的基本方法。
	- ② 基本方法PrimitiveOperation：是整个算法中的一个步骤，包含以下几种类型。 
		- 抽象方法：在抽象类中申明，由具体子类实现。
		- 具体方法：在抽象类中已经实现，在具体子类中可以继承或重写它。
		- 钩子方法：在抽象类中已经实现，包括用于判断的逻辑方法和需要子类重写的空方法两种。
- 具体子类（Concrete Class）：实现抽象类中所定义的抽象方法和钩子方法，它们是一个顶级逻辑的一个组成步骤。


# 代码实现 #
----------
		package com.TemplatePattern.sample;
		
		//抽象模板, 定义并实现了一个模板方法.
		public abstract class AbstractClass {
		    //一些抽象行为, 放到子类去实现.
		    public abstract void PrimitiveOperation1();
		    public abstract void PrimitiveOperation2();
		
		    //模板方法, 给出了逻辑的骨架, 而逻辑的组成是一些相应的抽象操作, 它们都推迟到子类实现.
		    public void TemplateMethod(){
		        PrimitiveOperation1();
		        PrimitiveOperation2();
		    }
		}

----------
		package com.TemplatePattern.sample;
		
		public class ConcreteClassA extends AbstractClass {
		    @Override
		    public void PrimitiveOperation1() {
		        System.out.println("具体类A方法1实现");
		    }
		
		    @Override
		    public void PrimitiveOperation2() {
		        System.out.println("具体类B方法2实现");
		    }
		}

----------
		package com.TemplatePattern.sample;
		
		public class ConcreteClassB extends AbstractClass {
		    @Override
		    public void PrimitiveOperation1() {
		        System.out.println("具体类B方法1实现");
		    }
		
		    @Override
		    public void PrimitiveOperation2() {
		        System.out.println("具体类B方法2实现");
		    }
		}

----------
		package com.TemplatePattern;
		
		import com.TemplatePattern.sample.AbstractClass;
		import com.TemplatePattern.sample.ConcreteClassA;
		import com.TemplatePattern.sample.ConcreteClassB;
		
		public class Main {
		
		    public static void main(String[] args) {
			    //sample code
		        {
		            AbstractClass c;
		
		            c = new ConcreteClassA();
		            c.TemplateMethod();
		
		            c = new ConcreteClassB();
		            c.TemplateMethod();
		        }
		    }
		}

----------
**输出**

- 具体类A方法1实现
- 具体类B方法2实现
- 具体类B方法1实现
- 具体类B方法2实现

----------




