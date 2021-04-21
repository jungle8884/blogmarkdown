---
title: DesignPatternofSixPrinciples
categories:
  - 后端
  - 设计模式
tags:
  - 设计模式
author:
  - Jungle
date: 2021-04-09 21:38:52
---

# 设计模式的六大原则 

设计模式的世界丰富多彩，比如生产一个个“产品”的工厂模式，衔接两个不相关接口的适配器模式，用不同的方式做同一件事的策略模式，构建步骤稳定、根据构建过程的不同配置构建出不同对象的建造者模式等等。

无论何种设计模式，都是基于六大设计原则：

## 1、开闭原则：

> 一个软件实体如类、模块和函数应该对修改封闭，对扩展开放。
>
> Software entities like classes, modules and functions should be open for extension but closed for modification

**[Open Closed Principle，OCP]**

- 当应用的需求改变时, 在不修改源代码的前提下, 可以扩展模块的功能, 使其满足新的需求.
- 遵守开闭原则的软件，其稳定性高和延续性强，从而易于扩展和维护.



## 2、单一职责原则：

> 一个类只做一件事，一个类应该只有一个引起它修改的原因。
>
> There should never be more than one reason for a class to change.

**[Single Responsibility Principle, SRP]**

**简单来说，一个类中应该是一组相关性很高的函数、数据的封装。**

- 比如当要做一个图片加载器的时候，不应该把所有的东西都写在一个类中，
- 应该各个功能独立出来，可以分成图片加载功能和缓存功能等模块，
- 这样类中的代码逻辑清晰可读性、可扩展性和可维护性会大大提高。



## 3、里氏替换原则：

> 所有引用基类的地方必须能透明地使用其子类的对象
>
> Functions that use use pointers or references to base classes must be able to use objects of derived classes without knowing it.

**[Liskov Substitution Principle, LSP]**

- 子类应该可以完全替换父类。

- 也就是说在使用继承时，只扩展新功能，而不要破坏父类原有的功能。

  



## 4、依赖倒置原则：

> 1、细节应该依赖于抽象，抽象不应依赖于细节。
> 2、把抽象层放在程序设计的高层，并保持稳定，程序的细节变化由低层的实现层来完成。
>
> High level modules should not depend upon low level modules. Both should depend upon abstractions.
>  Abstractions should not depend upon details. Details should depend upon abstractions.

**[Dependence Inversion Principle]**

- 要面向接口编程，不要面向实现编程
- 抽象层则相对稳定，因此以抽象为基础搭建起来的架构要比以细节为基础搭建起来的架构要稳定得多
- 抽象指的是接口或者抽象类，而细节是指具体的实现类
- 抽象就是一种制定的标准
- 依赖倒置原则的目的是通过要面向接口的编程来降低类间的耦合性:
  1. 每个类尽量提供接口或抽象类，或者两者都具备
  2. 变量的声明类型尽量是接口或者是抽象类
  3. 任何类都不应该从具体类派生
  4. 使用继承时尽量遵循里氏替换原则



## 5、迪米特法则：

> 一个类不应知道自己操作的类的细节，换言之，只和朋友谈话，不和朋友的朋友谈话。
>
> Talk only to your immediate friends and not to strangers

**[Demeter Principle]**

- 迪米特法则，又名“最少知道原则”
- 如果两个软件实体无须直接通信，那么就不应当发生直接的相互调用，可以通过第三方转发该调用。
- 目的是降低类之间的耦合度，提高模块的相对独立性.



## 6、接口隔离原则：

> 客户端不应依赖它不需要的接口。
>
> 如果一个接口在实现时，部分方法由于冗余被客户端空实现，则应该将接口拆分，让实现类只需依赖自己需要的接口方法。

**[Interface Segregation Principle]**

- 要为各个类建立它们需要的专用接口，而不要试图去建立一个很庞大的接口供所有依赖它的类去调用.
- 提高系统的灵活性和可维护性。
- 提高了系统的内聚性，减少了对外交互，降低了系统的耦合性
- 使用多个专门的接口还能够体现对象的层次，因为可以通过接口的继承，实现对总接口的定义
- 在具体应用接口隔离原则时，应该根据以下几个规则来衡量
  1. 接口尽量小，但是要有限度。 一个接口只服务于一个子模块或业务逻辑.
  2. 为依赖接口的类定制服务。 只提供调用者需要的方法，屏蔽不需要的方法.
  3. 提高内聚，减少对外交互。 使接口用最少的方法去完成最多的事情.

# 设计模式的总结

## 1、构建型模式

- 工厂方法模式：为每一类对象建立工厂，将对象交由工厂创建，客户端只和工厂打交道。
- 抽象工厂模式：为每一类工厂提取出抽象接口，使得新增工厂、替换工厂变得非常容易。
- 建造者模式：用于创建构造过程稳定的对象，不同的 Builder 可以定义不同的配置。
- 单例模式：全局使用同一个对象，分为饿汉式和懒汉式。懒汉式有双检锁和内部类两种实现方式。
- 原型模式：为一个类定义 clone 方法，使得创建相同的对象更方便。