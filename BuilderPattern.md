---
title: 建造者模式
categories:
  - 设计模式
tags:
  - 创建型模式
date: 2020-03-17 11:10:20
author: Jungle

---

**比如，汽车的创建。一台汽车由多个部件组成，车架、发动机、方向盘、轮胎等等，如何选取不同的组件？如何将这些组件组装成最终车辆？这不是也不应是对象的使用者需要考虑的问题。
一些产品都是由多个部件构成的，各个部件可以灵活选择，但其创建步骤都大同小异。这类产品的创建无法用前面介绍的工厂模式描述，而建造者模式可以很好地描述该类产品的创建。**

1. 建造者（Builder）模式的定义：指将一个复杂对象的构造与它的表示分离，使同样的构建过程可以创建不同的表示，这样的设计模式被称为建造者模式。**它是将一个复杂的对象分解为多个简单的对象，然后一步一步构建而成。**它将变与不变相分离，即产品的组成部分是不变的，但每一部分是可以灵活选择的。

2. 主要优点:
	- 各个具体的建造者相互独立，有利于系统的扩展。
	- 客户端不必知道产品内部组成的细节，便于控制细节风险。

3. 缺点:
	- 产品的组成部分必须相同，这限制了其使用范围。
	- 如果产品的内部变化复杂，该模式会增加很多的建造者类。

4. 建造者（Builder）模式的结构:
	1. 产品角色（Product）：它是包含多个组成部件的复杂对象，由具体建造者来创建其各个部件。
	2. 抽象建造者（Builder）：它是一个包含创建产品各个子部件的抽象方法的接口，通常还包含一个返回复杂产品的方法 getResult()。
	3. 具体建造者(Concrete Builder）：实现 Builder 接口，完成复杂产品的各个部件的具体创建方法。
	4. 指挥者（Director）：它调用建造者对象中的部件构造与装配方法完成复杂对象的创建，在指挥者中不涉及具体产品的信息。

5. 模式的应用场景:
	- 建造者（Builder）模式创建的是复杂对象，其产品的各个部分经常面临着剧烈的变化，但将它们组合在一起的算法却相对稳定，所以它通常在以下场合使用：
		- 创建的对象较复杂，由多个部件构成，各部件面临着复杂的变化，但构件间的建造顺序是稳定的。
		- 创建复杂对象的算法独立于该对象的组成部分以及它们的装配方式，即产品的构建过程和最终的表示（形态）是独立的。

6. 示例代码: 
	

----------

		package com.BuilderPattern;
	
		//包含多个组成部件的复杂对象
		public class Product {
		    // 组件
		    private int PartA;
		    private int PartB;
		    private int PartC;
		
		    public void SetPartA(int a){
		        PartA = a;
		    }
		
		    public void SetPartB(int b){
		        PartB = b;
		    }
		
		    public void SetPartC(int c){
		        PartC = c;
		    }
		
		    public void Show(){
		        System.out.println("PartA=" + PartA + "; PartB=" + PartB + "; PartA=" + PartC);
		    }
		}
	

----------

	
		package com.BuilderPattern;
	
		//一个包含创建产品各个子部件的抽象方法的接口，通常还包含一个返回复杂产品的方法 getResult()。
		public interface IBuilder {
		    void BuildPartA();
		    void BuildPartB();
		    void BuildPartC();
		    Product getResult();
		}

----------
		package com.BuilderPattern;
		
		//实现 Builder 接口，完成复杂产品的各个部件的具体创建方法。
		public class ConcreteBuilder1 implements IBuilder {
		
		    private Product product = new Product();
		
		    public void BuildPartA() {
		        product.SetPartA(1);
		    }
		
		    public void BuildPartB() {
		        product.SetPartB(2);
		    }
		
		    public void BuildPartC() {
		        product.SetPartC(3);
		    }
		
		    public Product getResult() {
		        return product;
		    }
		}

----------
		package com.BuilderPattern;
	
		public class ConcreteBuilder2 implements IBuilder{
		    private Product product = new Product();
		
		    public void BuildPartA() {
		        product.SetPartA(10);
		    }
		
		    public void BuildPartB() {
		        product.SetPartB(20);
		    }
		
		    public void BuildPartC() {
		        product.SetPartC(30);
		    }
		
		    public Product getResult() {
		        return product;
		    }
		}

----------

		package com.BuilderPattern;
		
		//调用建造者对象中的部件构造与装配方法完成复杂对象的创建，在指挥者中不涉及具体产品的信息。
		public class Director {
		
		    private IBuilder builder;
		
		    public Director(IBuilder builder){
		        this.builder = builder;
		    }
		
		    public void SetDirector(IBuilder builder){
		        this.builder = builder;
		    }
		
		    public Product construct(){
		        builder.BuildPartA();
		        builder.BuildPartB();
		        builder.BuildPartC();
		        return builder.getResult();
		    }
		
		}

----------

		package com.BuilderPattern;
	
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        IBuilder builder = new ConcreteBuilder1();
		        Director director = new Director(builder);
		        Product product = director.construct();
		        product.Show();
		
		        builder = new ConcreteBuilder2();
		        director = new Director(builder);
		        product = director.construct();
		        product.Show();
		    }
		}

----------

7. 注意事项：
	- 建造者（Builder）模式在应用过程中可以根据需要改变，如果创建的产品种类只有一种，只需要一个具体建造者，这时可以省略掉抽象建造者，甚至可以省略掉指挥者角色。
	- 建造者（Builder）模式和工厂模式的关注点不同：建造者模式注重零部件的组装过程，而工厂方法模式更注重零部件的创建过程，但两者可以结合使用。建造者可以使用工厂方法模式或抽象工厂模式创建部件；工厂方法模式或抽象工厂模式也可以使用建造者模式生产产品。
