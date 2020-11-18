---
title: 备忘录模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-06-02 11:56:53
author: Jungle

---
# 引言 #

- 每个人都有犯错误的时候，都希望有种“后悔药”能弥补自己的过失。
- 在计算机应用中，客户同样会常常犯错误，能否提供“后悔药”给他们呢？当然是可以的，而且是有必要的。这个功能由“备忘录模式”来实现。
- 其实很多应用软件都提供了这项功能，如 Word、记事本、Photoshop、Eclipse 等软件在编辑时按 Ctrl+Z 组合键时能撤销当前操作，使文档恢复到之前的状态；还有在数据库事务管理中的回滚操作、玩游戏时的中间结果存档功能、数据库与操作系统的备份操作、棋类游戏中的悔棋功能等都属于这类。
- 备忘录模式能记录一个对象的内部状态，当用户后悔时能撤销当前操作，使数据恢复到它原先的状态。

# 定义 #
- 在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便以后当需要时能将该对象恢复到原先保存的状态。该模式又叫快照模式。

# 优点 #
- 提供了一种可以恢复状态的机制。
	- 当用户需要时能够比较方便地将数据恢复到某个历史的状态。
- 实现了内部状态的封装。
	- 除了创建它的发起人之外，其他对象都不能够访问这些状态信息。
- 简化了"发起人"类。
	- 发起人不需要管理和保存其内部状态的各个备份，所有状态信息都保存在备忘录中，并由管理者进行管理，这符合单一职责原则。

# 缺点 #
资源消耗大。如果要保存的内部状态信息过多或者特别频繁，将会占用比较大的内存资源。

# 使用场景 #
- 需要保存与恢复数据的场景。
- 需要提供一个可回滚操作的场景。

# 结构 #
- 发起人（Originator）角色：记录当前时刻的内部状态信息，提供创建备忘录和恢复备忘录数据的功能，实现其他业务功能，它可以访问备忘录里的所有信息。
- 备忘录（Memento）角色：负责存储发起人的内部状态，在需要的时候提供这些内部状态给发起人。
- 管理者（Caretaker）角色：对备忘录进行管理，提供保存与获取备忘录的功能，但其不能对备忘录的内容进行访问与修改。

# 代码实现 #
----------
		package com.MementoPattern.sample;
		
		//备忘录类
		public class Memento {
		    //记录
		    private String state;
		
		    //构造方法, 将相关数据导入.
		    public Memento(String state){
		        this.state = state;
		    }
		
		    public String getState() {
		        return state;
		    }
		}

----------
		package com.MementoPattern.sample;
		
		//管理者类
		public class Caretaker {
		    private Memento memento;
		
		    public Memento getMemento() {
		        return memento;
		    }
		
		    public void setMemento(Memento memento) {
		        this.memento = memento;
		    }
		}
	

----------
		package com.MementoPattern.sample;
		
		//发起人类
		public class Originator {
		    //记录
		    private String state;
		    public void setState(String state) {
		        this.state = state;
		    }
		    public String getState() {
		        return state;
		    }
		
		    //创建备忘录, 将当前需要保存的信息导入并实例化出一个Memento对象
		    public Memento CreateMemento(){
		        return new Memento(state);
		    }
		
		    //恢复备忘录, 将Memento导入并将相关数据恢复
		    public void SetMemento(Memento memento){
		        state = memento.getState();
		    }
		
		    //显示数据
		    public void Show(){
		        System.out.println("State=" + state);
		    }
		}

----------
**输出**

- State=On
- State=Off
- State=On

----------

