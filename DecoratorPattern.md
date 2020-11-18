---
title: 装饰器模式
categories:
  - 设计模式
tags:
  - 结构型模式
date: 2020-04-28 23:11:44
author: Jungle

---

**说明: 代码参考自大话设计模式和老师上课的文档, 有些许改动.**

# 装饰器模式（Decorator Pattern） #

	- 在现实生活中，常常需要对现有产品增加新的功能或美化其外观，如房子装修、相片加相框等。
	- 在现实生活及软件开发过程中，有时想用一些现存的组件。
	- 这些组件可能只是完成了一些核心功能。
	- 但在不改变其结构的情况下，可以动态地扩展其功能。
	- 所有这些都可以釆用装饰模式来实现。
	- 通常情况下，扩展一个类的功能会使用继承方式来实现。但继承具有静态特征，耦合度高，并且随着扩展功能的增多，子类会很膨胀。
	- 如果使用组合关系来创建一个包装对象（即装饰对象）来包裹真实对象，并在保持真实对象的类结构不变的前提下，为其提供额外的功能，这就是装饰模式的目标。
	
## 定义 ##
	
	- 指在不改变现有对象结构的情况下，动态地给该对象增加一些职责（即增加其额外功能）和属性的模式，
	- 它属于对象结构型模式。

## 主要优点 ##

	- 采用装饰模式扩展对象的功能比采用继承方式更加灵活。
	- 可以设计出多个不同的具体装饰类，创造出多个不同行为的组合。

## 主要缺点 ##

	- 装饰模式增加了许多子类，如果过度使用会使程序变得很复杂。

## 应用场景 ##
	- 当需要给一个现有类添加附加职责，而又不能采用生成子类的方法进行扩充时。例如，该类被隐藏或者该类是终极类或者采用继承方式会产生大量的子类。
	- 当需要通过对现有的一组基本功能进行排列组合而产生非常多的功能时，采用继承关系很难实现，而采用装饰模式却很好实现。
	- 当对象的功能要求可以动态地添加，也可以再动态地撤销时。

----------

# 装饰模式结构 #

	1. 抽象构件（Component）角色：定义一个抽象接口以规范准备接收附加责任的对象。
	2. 具体构件（Concrete  Component）角色：实现抽象构件，通过装饰角色为其添加一些职责。
	3. 抽象装饰（Decorator）角色：继承抽象构件，并包含具体构件的实例，可以通过其子类扩展具体构件的功能。
	4. 具体装饰（ConcreteDecorator）角色：实现抽象装饰的相关方法，并给具体构件对象添加附加的责任。

----------

# 代码实现 #
## 理论代码 ##

		package com.DecoratorPattern;
		
		interface Component {
		    public void operation();
		}

----------
		package com.DecoratorPattern;
		
		class ConcreteComponent implements Component{
		    @Override
		    public void operation() {
		        System.out.println("具体对象的操作.");
		    }
		}


----------
		package com.DecoratorPattern;
		
		class Decorator implements Component{
		    private Component component;
		    public void setComponent(Component component){
		        this.component = component;
		    }
		    @Override
		    public void operation() {
		        if(component != null)
		            component.operation();
		    }
		}

----------
		package com.DecoratorPattern;
		
		class ConcreteDecoratorA extends Decorator{
		    private String addedState; //本类独有功能,  以区别于ConcreteDecoratorB
		
		    @Override
		    public void operation() {
		        super.operation();
		        addedState = "New State";
		        System.out.println("具体装饰对象A的操作");
		    }
		}

----------
		package com.DecoratorPattern;
		
		class ConcreteDecoratorB extends Decorator {
		    @Override
		    public void operation() {
		        super.operation();
		        AddedBehavior();
		        System.out.println("具体装饰对象B的操作");
		    }
		
		    private void AddedBehavior(){
		        // 本类独有方法, 以区别于ConcreteDecoratorA
		        System.out.println("具体装饰对象B增加额外的功能");
		    }
		}

----------
		package com.DecoratorPattern;
		import example.*;
		
		public class Main {
		
		    public static void main(String[] args) {

		        {
		            ConcreteComponent c = new ConcreteComponent();
		            ConcreteDecoratorA d1 = new ConcreteDecoratorA();
		            ConcreteDecoratorB d2 = new ConcreteDecoratorB();
		
		            d1.setComponent(c);
		            d2.setComponent(d1);
		            d2.operation();
		        }
		     
		    }
		}


----------

----------
## 结合具体例子实现 ##

		package example;
		/*
		* 如果只有一个ConcreteComponent类而没有抽象的Component类,
		*  那么Decorator类可以是ConcreteComponent的一个子类.
		* 同理, 如果只有一个ConcreteDecorator类, 那么就没有必要建立一个单独的Decorator类,
		*  而可以把Decorator和ConcreteDecorator的责任合并成一个类.
		* */
		public class Person { // 相当于ConcreteComponent(没有抽象的Component类)
		    public Person(){}
		
		    private String name;
		    public Person(String name){
		        this.name = name;
		    }
		
		    public void Show(){
		        System.out.println("装扮的" + name);
		    }
		}

----------
		package example;
		
		public class Finery extends Person{ // 相当于Decorator
		    protected Person component;
		
		    public void Decorate(Person component){
		        this.component = component;
		    }
		
		    @Override
		    public void Show() {
		        if(component != null)
		            component.Show();
		    }
		}

----------
		package example;
		
		public class BigTrouser extends Finery{
		    public void Show(){
		        System.out.print("垮裤  ");
		        super.Show();
		    }
		}

----------
		package example;
		
		public class LeatherShoes extends Finery {
		    @Override
		    public void Show() {
		        System.out.print("皮鞋    ");
		        super.Show();
		    }
		}

----------
		package example;
		
		public class Sneakers extends Finery {
		    @Override
		    public void Show() {
		        System.out.print("破球鞋   ");
		        super.Show();
		    }
		}

----------
		package example;
		
		public class Suit extends Finery {
		    @Override
		    public void Show() {
		        System.out.print("西装    ");
		        super.Show();
		    }
		}

----------
		package example;
		
		public class Tie extends Finery {
		    @Override
		    public void Show() {
		        System.out.print("领带    ");
		        super.Show();
		    }
		}

----------
		package example;
		
		public class TShirts extends Finery {
		    @Override
		    public void Show() {
		        System.out.print("大T恤   ");
		        super.Show();
		    }
		}

----------
		package com.DecoratorPattern;
		import example.*;
		
		public class Main {
		
		    public static void main(String[] args) {
		        {
		            ConcreteComponent c = new ConcreteComponent();
		            ConcreteDecoratorA d1 = new ConcreteDecoratorA();
		            ConcreteDecoratorB d2 = new ConcreteDecoratorB();
		
		            d1.setComponent(c);
		            d2.setComponent(d1);
		            d2.operation();
		        }
		        System.out.println("\n---------------------");
		        {
		            Person xc = new Person("小蔡");
		            System.out.println("\n第一种装扮: ");
		            Sneakers pqx = new Sneakers();
		            BigTrouser kk = new BigTrouser();
		            TShirts dtx = new TShirts();
		
		            pqx.Decorate(xc);
		            kk.Decorate(pqx);
		            dtx.Decorate(kk);
		            dtx.Show();
		
		            System.out.println("\n第二种装扮: ");
		            LeatherShoes px = new LeatherShoes();
		            Tie ld = new Tie();
		            Suit xz = new Suit();
		
		            px.Decorate(xc);
		            ld.Decorate(px);
		            xz.Decorate(ld);
		            xz.Show();
		
		            System.out.println("\n第三种装扮: ");
		            Sneakers pqx2 = new Sneakers();
		            LeatherShoes px2 = new LeatherShoes();
		            BigTrouser kk2 = new BigTrouser();
		            Tie ld2 = new Tie();
		
		            pqx2.Decorate(xc);
		            px2.Decorate(pqx2);
		            kk2.Decorate(px2);
		            ld2.Decorate(kk2);
		            ld2.Show();
		        }
		    }
		}

----------
**代码下载链接: https://github.com/jungle8884/DecoratorPattern**
