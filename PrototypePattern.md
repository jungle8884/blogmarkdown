---
title: 原型模式
categories:
  - 设计模式
tags:
  - 创建型模式
date: 2020-03-18 15:33:02
author: Jungle

---

1. **引言**

	- 工厂方法模式和抽象工厂模式描述了由工厂在工厂方法中创建对象的形式
	- 建造者模式描述了由指定的建造者进行对象搭建的方法
	- 单例模式描述了由类自身创建对象保证只有唯一一个对象可以存在
	- 原型模式（Prototype Pattern）描述了由类对象复制自身而生成对象的方法: 在有些系统中，存在大量相同或相似对象的创建问题，如果用传统的构造函数来创建对象，会比较复杂且耗时耗资源，用原型模式生成对象就很高效


**原型模式（Prototype Pattern）是用于创建重复的对象，同时又能保证性能。这种模式实现了一个原型接口，该接口用于创建当前对象的克隆。当直接创建对象的代价比较大时，则采用这种模式。
另外，有时会向已完成的系统中添加新的成员，该成员实现了已知接口。我们希望在不改变原系统的情况下自由使用该成员。如一些游戏的插件，高级游戏玩家喜欢自己编写插件加入到游戏当中。这些插件符合原系统接口规范，但并不需要原系统知道这些新插件的类名字。可以用原型模式实现。**

2. 原型（Prototype）模式的**定义**：用一个已经创建的实例作为原型，通过复制该原型对象来创建一个和原型相同或相似的新对象。在这里，原型实例指定了要创建的对象的种类。用这种方式创建对象非常高效，根本无须知道对象创建的细节。
3. 模式的结构:
	1. 抽象原型类：规定了具体原型对象必须实现的接口。
	2. 具体原型类：实现抽象原型类的 clone() 方法，它是可被复制的对象。
	3. 访问类：使用具体原型类中的 clone() 方法来复制新的对象。

4. 示例代码: 
	

----------
		package com.PrototypePattern;
		
		public class Area {
		
		    // 钞票单位
		    private String unit;
		
		    public String getUnit(){
		        return unit;
		    }
		
		    public void setUnit(String unit){
		        this.unit = unit;
		    }
		}

----------
		package com.PrototypePattern;
		
		/*
		* 由于 Java 提供了对象的 clone() 方法，所以用 Java 实现原型模式很简单。
		* Object对象有个clone()方法，实现了对象中各个属性的复制，但它的可见范围是protected的，
		* 所以实体类使用克隆的前提是：
		*       实现Cloneable接口，这是一个标记接口，自身没有方法。
		*       覆盖clone()方法，可见性提升为public。
		* */
		
		//具体原型类
		public class Money implements Cloneable {
		
		    // 面值
		    private int faceValue;
		
		    private Area area;
		
		    public int getFaceValue(){
		        return faceValue;
		    }
		
		    public void setFaceValue(int faceValue){
		        this.faceValue = faceValue;
		    }
		
		    public Money(int faceValue, Area area){
		        this.faceValue = faceValue;
		        this.area = area;
		    }
		
		    public Area getArea(){
		        return area;
		    }
		
		    public void setArea(Area area) {
		        this.area = area;
		    }
		
		    // 覆盖clone()方法，可见性提升为public
		    // 浅克隆，即只是进行了值复制，对于只包含基本类型属性的类，这种克隆没有问题。
		    // 但如果一个对象除了包含基本类型属性，还包含其它实体类对象的引用，
		    // 这种浅克隆只是复制了对象的引用地址，被克隆的对象中的引用对象与原引用对象共享相同的引用。
		    public Money clone() throws CloneNotSupportedException{
		        //super.clone() 的调用就是沿着继承树不断往上递归调用直到Object 的clone方法
		        //Object.clone()根据当前对象的类型创建一个新的同类型的空对象，
		        //然后把当前对象的字段的值逐个拷贝到新对象上，然后返回给上一层clone() 调用
		        //也就是说super.clone() 的浅复制效果是通过Object.clone()实现的
		        return (Money)super.clone();
		    }
		
		//    // 要实现将克隆对象内容（包含其它实体类对象的引用）完全复制，就需进行深克隆，将各引用的对象也专门复制。
		//    public  Money clone() throws CloneNotSupportedException{
		//        //填加自身向money复制各引用对象的代码
		//        Money money = (Money)super.clone();
		//        return money;
		//    }
		}


----------
package com.PrototypePattern;

public class Main {

    public static void main(String[] args) {
	// write your code here
        Area area = new Area();
        area.setUnit("RMB");

        // 原型实例，100RMB的钞票
        Money money = new Money(100, area);

        for (int i = 1; i <= 3; i++) {
            try {
                Money cloneMoney = money.clone();
                cloneMoney.setFaceValue(i * 100);
                System.out.println("这张是" + cloneMoney.getFaceValue() +  cloneMoney.getArea().getUnit() + "的钞票");
            } catch (CloneNotSupportedException e) {
                e.printStackTrace();
            }
        }
    }
}

	输出:
		这张是100RMB的钞票
		这张是200RMB的钞票
		这张是300RMB的钞票
----------
**注**:

	- 如果不重写clone()方法，则在调用clone()方法实现的是浅复制（所有的引用对象保持不变，意思是如果原型里这些对象发生改变会直接影响到复制对象）。
	- 重写clone()方法，一般会先调用super.clone()进行浅复制，然后再复制那些易变对象，从而达到深复制的效果。
		- super.clone()，这个操作主要是来做一次bitwise copy( binary copy )，即浅拷贝，他会把原对象完整的拷贝过来包括其中的引用。这样会带来问题，如果里面的某个属性是个可变对象，那么原来的对象改变，克隆的对象也跟着改变。所以在调用完super.clone()后，一般还需要重新拷贝可变对象。
	- 调用super.clone()后返回的对象满足以下:
		- x.clone != x
		- x.clone.getClass() == x.getClass()
		- 也就是说JavaDoc指明了Object.clone()有特殊的语义，他就是能把当前对象的整个结构完全浅拷贝一份出来。
		- 每层clone()都顺着 super.clone() 的链向上调用的话最终就会来到Object.clone() ，于是根据上述的特殊语义就可以有 x.clone.getClass() == x.getClass() 。

[本小节参考自: https://www.jianshu.com/p/6ee09071f74d]

