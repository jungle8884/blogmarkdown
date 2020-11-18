---
title: 组合模式
categories:
  - 设计模式
tags:
  - 结构型模式
date: 2020-03-21 12:42:00
author: Jungle

---
# 引言 #
	- 在现实生活及软件开发过程中，存在很多“部分-整体”的关系，
	- 例如，大学中学院与专业和班级、
	- 文件系统中的文件与文件夹、
	- 窗体程序中的简单控件与容器控件、
	- 绘图是的图形组合等。
	- 公司总部和分公司
	- 对这些简单对象与复合对象的处理，如果用组合模式来实现会很方便

# 定义 #
	- 它是一种将对象组合成树状的层次结构的模式，用来表示“部分-整体”的关系，使用户对单个对象和组合对象具有一致的访问性

# 优点 #
	- 组合模式使得客户端代码可以一致地处理单个对象和组合对象，无须关心自己处理的是单个对象，还是组合对象.
	- 更容易在组合体内加入新的对象，客户端不会因为加入了新的对象而更改源代码，满足“开闭原则”.

# 缺点 #
	- 设计较复杂，客户端需要花更多时间理清类之间的层次关系
	- 不容易限制容器中的构件
	- 不容易用继承的方法来增加构件的新功能

# 应用场景 #
	- 在需要表示一个对象整体与部分的层次结构的场合
	- 要求对用户隐藏组合对象与单个对象的不同，用户可以用统一的接口使用组合结构中的所有对象的场合

# 组合模式结构 #
	1. 抽象构件（Component）角色：它的主要作用是为树叶构件和树枝构件声明公共接口，并实现它们的默认行为
	2. 树叶构件（Leaf）角色：是组合中的叶节点对象，它没有子节点，用于实现抽象构件角色中声明的公共接口
	3. 树枝构件（Composite）角色：是组合中的分支节点对象，它有子节点。它实现了抽象构件角色中声明的接口，它的主要作用是存储和管理子部件，通常包含 Add()、Remove()、GetChild() 等方法

# 代码实现 #

----------
		package com.CompositePattern;

		abstract class Component {
		    protected String name;
		    public Component(String name){
		        this.name = name;
		    }
		
		    public void add(Component c){};
		    public void remove(Component c){};
		    public Component getChild(int i){return null;}
		    public void operation(){};
		}

----------
		package com.CompositePattern;
		
		public class Leaf extends Component {
		    public Leaf(String name){
		        super(name);
		    }
		
		    // 由于叶子节点没有再增加分枝和树叶, 所以add和remove没有意义.
		    @Override
		    public void add(Component c) {
		        System.out.println("不能增加叶子节点!");
		    }
		
		    @Override
		    public void remove(Component c) {
		        System.out.println("不能移除叶子节点!");
		    }
		
		    @Override
		    public Component getChild(int i) {
		        return null;
		    }
		
		    @Override
		    public void operation() {
		        System.out.println("树叶类:" + name + "!");
		    }
		}

----------
		package com.CompositePattern;
		
		import java.util.ArrayList;
		
		public class Composite extends Component{
		    private ArrayList<Component> children = new ArrayList<Component>();
		    public Composite(String name){
		        super(name);
		    }
		
		    @Override
		    public void add(Component c) {
		        children.add(c);
		    }
		
		    @Override
		    public void remove(Component c) {
		        children.remove(c);
		    }
		
		    @Override
		    public Component getChild(int i) {
		        return children.get(i);
		    }
		
		    @Override
		    public void operation() {
		        System.out.println("组件类" + name + "!");
		        for(Component c : children){
		            c.operation();
		        }
		    }
		}

----------
		package com.CompositePattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        Component c0 = new Composite("co");
		        Component c1 = new Composite("c1");
		        Component c2 = new Composite("c2");
		        Component c3 = new Composite("c3");
		        Component leaf1 = new Leaf("leaf1");
		        Component leaf2 = new Leaf("leaf2");
		        Component leaf3 = new Leaf("leaf3");
		        Component leaf4 = new Leaf("leaf4");
		        Component leaf5 = new Leaf("leaf5");
		        Component leaf6 = new Leaf("leaf6");
		
		        c0.add(c1);
		        c0.add(c2);
		
		        c1.add(leaf1);
		        c1.add(leaf2);
		        c1.add(c3);
		
		        c2.add(leaf3);
		        c2.add(leaf4);
		
		        c3.add(leaf5);
		        c3.add(leaf6);
		
		        c0.operation();
		    }
		}

输出: 
		组件类co!
		组件类c1!
		树叶类:leaf1!
		树叶类:leaf2!
		组件类c3!
		树叶类:leaf5!
		树叶类:leaf6!
		组件类c2!
		树叶类:leaf3!
		树叶类:leaf4!

----------
**代码下载:** https://github.com/jungle8884/CompositePattern