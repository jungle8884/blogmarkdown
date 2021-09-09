---
title: 六大设计原则
categories:
  - 后端
  - 设计模式
tags:
  - 设计模式
author:
  - Jungle
date: 2021-04-09 
---

# 设计模式的六大原则 

设计模式的世界丰富多彩，比如生产一个个“产品”的工厂模式，衔接两个不相关接口的适配器模式，用不同的方式做同一件事的策略模式，构建步骤稳定、根据构建过程的不同配置构建出不同对象的建造者模式等等。

无论何种设计模式，都是基于六大设计原则：

![image-20210907122926155](DesignPatternofSixPrinciples/image-20210907122926155.png)

## 1、开闭原则：

> 一个软件实体如类、模块和函数应该对 **`修改封闭，对扩展开放`**。
>
> Software entities like classes, modules and functions should be open for extension but closed for modification

**[Open Closed Principle，OCP]**

- 当应用的需求改变时, 在不修改源代码的前提下, 可以扩展模块的功能, 使其满足新的需求.
- 遵守开闭原则的软件，其稳定性高和延续性强，从而易于扩展和维护.



## 2、单一职责原则：

> `一个类只做一件事，一个类应该只有一个引起它修改的原因`。
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

> 1、**`细节应该依赖于抽象`**，抽象不应依赖于细节。
> 2、把抽象层放在程序设计的高层，并保持稳定，程序的细节变化由低层的实现层来完成。
>
> High level modules should not depend upon low level modules. Both should depend upon abstractions.
>  Abstractions should not depend upon details. Details should depend upon abstractions.

**[Dependence Inversion Principle]**

- 要面向接口编程，不要面向实现编程
- 抽象层则相对稳定，因此以抽象为基础搭建起来的架构要比以细节为基础搭建起来的架构要稳定得多
- `抽象指的是接口或者抽象类，而细节是指具体的实现类`
- 抽象就是一种制定的标准
- 依赖倒置原则的目的是通过要面向接口的编程来降低类间的耦合性:
  1. `每个类尽量提供接口或抽象类`，或者两者都具备
  2. `变量的声明类型尽量是接口或者是抽象类`
  3. `任何类都不应该从具体类派生`
  4. `使用继承时尽量遵循里氏替换原则`



## 5、迪米特法则：

> **`一个类不应知道自己操作的类的细节`**，换言之，只和朋友谈话，不和朋友的朋友谈话。
>
> Talk only to your immediate friends and not to strangers

**[Demeter Principle]**

- 迪米特法则，又名“最少知道原则”
- 如果两个软件实体无须直接通信，那么就不应当发生直接的相互调用，可以通过第三方转发该调用。
- 目的是降低类之间的耦合度，提高模块的相对独立性.



## 6、接口隔离原则：

> **``客户端不应依赖它不需要的接口`。**
>
> 如果一个接口在实现时，部分方法由于冗余被客户端空实现，则应该将接口拆分，让实现类只需依赖自己需要的接口方法。

**[Interface Segregation Principle]**

- 要为各个类建立它们需要的专用接口，而不要试图去建立一个很庞大的接口供所有依赖它的类去调用.
- 提高系统的灵活性和可维护性。
- 提高了系统的内聚性，减少了对外交互，降低了系统的耦合性
- 使用多个专门的接口还能够体现对象的层次，因为可以通过接口的继承，实现对总接口的定义
- 在具体应用接口隔离原则时，应该根据以下几个规则来衡量
  1. `接口尽量小`，但是要有限度。 `一个接口只服务于一个子模块或业务逻辑`.
  2. 为依赖接口的类定制服务。 只提供调用者需要的方法，屏蔽不需要的方法.
  3. `提高内聚`，减少对外交互。 使接口用最少的方法去完成最多的事情.



# [几种常见的UML图](https://www.jianshu.com/p/57620b762160)

## 1 继承： 空心三角形+实线 `is-a`

![image-20210907132016968](DesignPatternofSixPrinciples/image-20210907132016968.png)



## 2 实现： 空心三角形+虚线

![image-20210907132140394](DesignPatternofSixPrinciples/image-20210907132140394.png)

## 3 依赖： 虚线箭头

![image-20210907132219875](DesignPatternofSixPrinciples/image-20210907132219875.png)

## 4 关联： 实现箭头

![image-20210907132307753](DesignPatternofSixPrinciples/image-20210907132307753.png)

## 5 聚合： 空心的菱形+实现箭头 `has-a 弱拥有`

![image-20210907132408072](DesignPatternofSixPrinciples/image-20210907132408072.png)

## 6 组合： 实心菱形+实现箭头 `contains-a`

![image-20210907132536420](DesignPatternofSixPrinciples/image-20210907132536420.png)



---

# 设计模式的总结

## 一、构建型模式 ：`Creational Pattern`

### [简单工厂模式](https://leetcode-cn.com/leetbook/read/design-patterns/99gpi3/): `SimpleFactory`

> 让一个工厂类承担构建所有对象的职责。调用者需要什么产品，让工厂生产出来即可。

- 违背了单一职责原则
- 开闭原则

![image-20210907124257542](DesignPatternofSixPrinciples/image-20210907124257542.png)

### [工厂方法模式](https://leetcode-cn.com/leetbook/read/design-patterns/99gpi3/)：`Factory`

> 为每一类对象建立工厂，将对象交由工厂创建，客户端只和工厂打交道。

- 调用者无需知道苹果的生产细节，当生产过程需要修改时也无需更改调用端。
- 同时，工厂方法模式解决了简单工厂模式的两个弊端。
  - 符合单一职责原则
  - 保持了面向对象的可扩展性，符合开闭原则。

![image-20210907124522384](DesignPatternofSixPrinciples/image-20210907124522384.png)

![image-20210907124603578](DesignPatternofSixPrinciples/image-20210907124603578.png)

### [抽象工厂模式](https://leetcode-cn.com/leetbook/read/design-patterns/99zelm/)：`Abstract Factory`

> 为每一类工厂提取出抽象接口，使得新增工厂、替换工厂变得非常容易。

- 抽象工厂模式很好的发挥了开闭原则、依赖倒置原则，但缺点是抽象工厂模式太重了，如果 IFactory 接口需要新增功能，则会影响到所有的具体工厂类。
- 使用抽象工厂模式，替换具体工厂时只需更改一行代码，但要新增抽象方法则需要修改所有的具体工厂类。
- 所以抽象工厂模式适用于增加同类工厂这样的横向扩展需求，不适合新增功能这样的纵向扩展。

![image-20210907125015444](DesignPatternofSixPrinciples/image-20210907125015444.png)

![image-20210907125052963](DesignPatternofSixPrinciples/image-20210907125052963.png)

---

### [建造者模式](https://leetcode-cn.com/leetbook/read/design-patterns/99sjd7/)：`Builder`

> 用于**创建构造过程稳定**的对象，不同的 Builder 可以定义不同的配置。

```java
public class MilkTea {
    private final String type;
    private final String size;
    private final boolean pearl;
    private final boolean ice;

	private MilkTea() {}
	
    private MilkTea(Builder builder) {
        this.type = builder.type;
        this.size = builder.size;
        this.pearl = builder.pearl;
        this.ice = builder.ice;
    }

    public String getType() {
        return type;
    }

    public String getSize() {
        return size;
    }

    public boolean isPearl() {
        return pearl;
    }
    public boolean isIce() {
        return ice;
    }

    public static class Builder {

        private final String type;
        private String size = "中杯";
        private boolean pearl = true;
        private boolean ice = false;

        public Builder(String type) {
            this.type = type;
        }

        public Builder size(String size) {
            this.size = size;
            return this;
        }

        public Builder pearl(boolean pearl) {
            this.pearl = pearl;
            return this;
        }

        public Builder ice(boolean cold) {
            this.ice = cold;
            return this;
        }

        public MilkTea build() {
            return new MilkTea(this);
        }
    }
}
/*
    可以看到，我们将 MilkTea 的构造方法设置为私有的，
    所以外部不能通过 new 构建出 MilkTea 实例，只能通过 Builder 构建。
    对于必须配置的属性，通过 Builder 的构造方法传入，可选的属性通过 Builder 的链式调用方法传入，
    如果不配置，将使用默认配置，也就是中杯、加珍珠、不加冰。根据不同的配置可以制作出不同的奶茶：
*/

public class User {
    private void buyMilkTea() {
        MilkTea milkTea = new MilkTea.Builder("原味").build();
        show(milkTea);

        MilkTea chocolate =new MilkTea.Builder("巧克力味")
                .ice(false)
                .build();
        show(chocolate);
        
        MilkTea strawberry = new MilkTea.Builder("草莓味")
                .size("大杯")
                .pearl(false)
                .ice(true)
                .build();
        show(strawberry);
    }

    private void show(MilkTea milkTea) {
        String pearl;
        if (milkTea.isPearl())
            pearl = "加珍珠";
        else
            pearl = "不加珍珠";
        String ice;
        if (milkTea.isIce()) {
            ice = "加冰";
        } else {
            ice = "不加冰";
        }
        System.out.println("一份" + milkTea.getSize() + "、"
                + pearl + "、"
                + ice + "的"
                + milkTea.getType() + "奶茶");
    }
}
```

- 单例模式：全局使用同一个对象，分为饿汉式和懒汉式。懒汉式有双检锁和内部类两种实现方式。

- [原型模式](https://leetcode-cn.com/leetbook/read/design-patterns/994yw5/)：为一个类定义 clone 方法，使得创建相同的对象更方便。(就是用来克隆对象的)

  ![image-20210907130130836](DesignPatternofSixPrinciples/image-20210907130130836.png)

  ![image-20210907130154639](DesignPatternofSixPrinciples/image-20210907130154639.png)

  ![image-20210907130214927](DesignPatternofSixPrinciples/image-20210907130214927.png)



---

## 二、结构型模式 `Structural Pattern`

### [适配器模式](https://leetcode-cn.com/leetbook/read/design-patterns/99yk22/)：`Adapter`

> 用于有相关性但不兼容的接口

- 但适配器模式并不推荐多用。
- 因为未雨绸缪好过亡羊补牢，
  - 如果事先能预防接口不同的问题，不匹配问题就不会发生，
  - 只有遇到源接口无法改变时，才应该考虑使用适配器。
  - 比如现代的电源插口中很多已经增加了专门的 USB 充电接口，让我们不需要再使用适配器转换接口，这又是社会的一个进步。

![image-20210907130900266](DesignPatternofSixPrinciples/image-20210907130900266.png)

![image-20210907130938153](DesignPatternofSixPrinciples/image-20210907130938153.png)

![image-20210907131023949](DesignPatternofSixPrinciples/image-20210907131023949.png)

---

### [桥接模式](https://leetcode-cn.com/leetbook/read/design-patterns/99c3mc/)：`Bridge`

> 用于同等级的接口互相**组合**

- 这时我们再来回顾一下官方定义：将抽象部分与它的实现部分分离，使它们都可以独立地变化。
- 抽象部分指的是父类，对应本例中的形状类，实现部分指的是不同子类的区别之处。
- 将子类的区别方式 —— 也就是本例中的颜色 —— 分离成接口，通过组合的方式桥接颜色和形状，这就是桥接模式，
- 它主要用于两个或多个同等级的接口。

![image-20210907131238337](DesignPatternofSixPrinciples/image-20210907131238337.png)

![image-20210907131259514](DesignPatternofSixPrinciples/image-20210907131259514.png)

**接下来我们有了新的需求，每种形状都需要有四种不同的颜色：红、蓝、黄、绿。**

![image-20210907131340071](DesignPatternofSixPrinciples/image-20210907131340071.png)

![image-20210907131410172](DesignPatternofSixPrinciples/image-20210907131410172.png)

**在每个形状类中，桥接 IColor 接口：**

```java
class Rectangle implements IShape {

    private IColor color;

    void setColor(IColor color) {
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.println("绘制" + color.getColor() + "矩形");
    }
}


class Round implements IShape {

    private IColor color;

    void setColor(IColor color) {
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.println("绘制" + color.getColor() + "圆形");
    }
}


class Triangle implements IShape {

    private IColor color;

    void setColor(IColor color) {
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.println("绘制" + color.getColor() + "三角形");
    }
}
```

![image-20210907131527022](DesignPatternofSixPrinciples/image-20210907131527022.png)

---

### [组合模式](https://leetcode-cn.com/leetbook/read/design-patterns/99bupi/)：`Composite`

> 用于整体与部分的结构

![image-20210907133004118](DesignPatternofSixPrinciples/image-20210907133004118.png)

![image-20210907133100865](DesignPatternofSixPrinciples/image-20210907133100865.png)

![image-20210907133133671](DesignPatternofSixPrinciples/image-20210907133133671.png)

![image-20210907133157682](DesignPatternofSixPrinciples/image-20210907133157682.png)

![image-20210907133220554](DesignPatternofSixPrinciples/image-20210907133220554.png)

![image-20210907133354949](DesignPatternofSixPrinciples/image-20210907133354949.png)

**`上面是透明方式实现`**

![image-20210907133412964](DesignPatternofSixPrinciples/image-20210907133412964.png)

**`下面是安全方式实现`**

![image-20210907133638489](DesignPatternofSixPrinciples/image-20210907133638489.png)

![image-20210907133651254](DesignPatternofSixPrinciples/image-20210907133651254.png)

![image-20210907133723757](DesignPatternofSixPrinciples/image-20210907133723757.png)

**接口隔离原则： 客户端不应依赖它不需要的接口**

![image-20210907133917386](DesignPatternofSixPrinciples/image-20210907133917386.png)

---

### [装饰器模式](https://leetcode-cn.com/leetbook/read/design-patterns/99j7re/): `Decorator`

> 动态地给一个对象增加一些额外的职责

- 增强一个类原有的功能
- 为一个类添加新的功能

![image-20210907134612083](DesignPatternofSixPrinciples/image-20210907134612083.png)

**用于增强功能的装饰模式**

![image-20210907143434396](DesignPatternofSixPrinciples/image-20210907143434396.png)

![image-20210907143446570](DesignPatternofSixPrinciples/image-20210907143446570.png)

![image-20210907143547857](DesignPatternofSixPrinciples/image-20210907143547857.png)

![image-20210907143622932](DesignPatternofSixPrinciples/image-20210907143622932.png)

> 可以看到，装饰器也实现了 IBeauty 接口，并且没有添加新的方法，也就是说这里的装饰器仅用于增强功能，并不会改变 Me 原有的功能，这种装饰模式称之为**透明装饰模式**，由于**没有改变接口，也没有新增方法**，所以透明装饰模式可以无限装饰。



**用于添加功能的装饰模式**

![image-20210907143940158](DesignPatternofSixPrinciples/image-20210907143940158.png)

![image-20210907144000654](DesignPatternofSixPrinciples/image-20210907144000654.png)

![image-20210907144313373](DesignPatternofSixPrinciples/image-20210907144313373.png)

![image-20210907144034462](DesignPatternofSixPrinciples/image-20210907144034462.png)

![image-20210907144543944](DesignPatternofSixPrinciples/image-20210907144543944.png)

![image-20210907144714128](DesignPatternofSixPrinciples/image-20210907144714128.png)

> 为什么叫半透明呢？由于新的接口 IStickyHookHouse 拥有之前 IHouse 不具有的方法，所以我们如果要使用装饰器中添加的功能，就不得不区别对待装饰前的对象和装饰后的对象。
>
> 也就是说客户端要使用新方法，
>
> - 必须知道具体的装饰类 StickyHookDecorator，
>   - 所以这个装饰类对客户端来说是**可见的**、不透明的。
> - 而被装饰者不一定要是 House，它可以是实现了 IHouse 接口的任意对象，
>   - 所以被装饰者对客户端是**不可见的**、透明的。
> - 由于一半透明，一半不透明，所以称之为半透明装饰模式。

![image-20210907144756529](DesignPatternofSixPrinciples/image-20210907144756529.png)



> **举例说明：I/O 中的装饰模式**

![image-20210907145145447](DesignPatternofSixPrinciples/image-20210907145145447.png)

![image-20210907145006628](DesignPatternofSixPrinciples/image-20210907145006628.png)

![image-20210907145251608](DesignPatternofSixPrinciples/image-20210907145251608.png)

![image-20210907145605672](DesignPatternofSixPrinciples/image-20210907145605672.png)

![image-20210907145836252](DesignPatternofSixPrinciples/image-20210907145836252.png)

![image-20210907150035628](DesignPatternofSixPrinciples/image-20210907150035628.png)

![image-20210907150240448](DesignPatternofSixPrinciples/image-20210907150240448.png)

![image-20210907150321309](DesignPatternofSixPrinciples/image-20210907150321309.png)



---

### [外观模式](https://leetcode-cn.com/leetbook/read/design-patterns/99fweg/)：`Facade`

> 体现封装的思想

![image-20210907150620004](DesignPatternofSixPrinciples/image-20210907150620004.png)

![image-20210907150821550](DesignPatternofSixPrinciples/image-20210907150821550.png)

![image-20210907150833137](DesignPatternofSixPrinciples/image-20210907150833137.png)

![image-20210907150854772](DesignPatternofSixPrinciples/image-20210907150854772.png)

![image-20210907150940830](DesignPatternofSixPrinciples/image-20210907150940830.png)

![image-20210907151013879](DesignPatternofSixPrinciples/image-20210907151013879.png)



---

### [享元模式]()：`Flyweight`

> 体现面向对象的可复用性
>
> [百度百科](https://baike.baidu.com/item/%E7%BB%86%E7%B2%92%E5%BA%A6/2061662): 细粒度模型,通俗的讲就是将业务模型中的对象加以细分,从而得到更科学合理的对象模型,直观的说就是划分出很多对象.
>
> [对象的粒度](https://blog.csdn.net/xkhzk2/article/details/84289252)：可以划分为子类的程度？
>
> ![image-20210907151436434](DesignPatternofSixPrinciples/image-20210907151436434.png)

![image-20210907151200662](DesignPatternofSixPrinciples/image-20210907151200662.png)



---

### [代理模式]()：`Proxy`

> 主要用于对某个对象加以控制

![image-20210907152301427](DesignPatternofSixPrinciples/image-20210907152301427.png)

![image-20210907152329467](DesignPatternofSixPrinciples/image-20210907152329467.png)

![image-20210907152453865](DesignPatternofSixPrinciples/image-20210907152453865.png)

![image-20210907152505981](DesignPatternofSixPrinciples/image-20210907152505981.png)

![image-20210907152603712](DesignPatternofSixPrinciples/image-20210907152603712.png)

**看实现目的：**

> 代理类看起来和装饰模式的 `FilterInputStream` 一模一样，但两者的目的不同，装饰模式是为了增强功能或添加功能，代理模式主要是为了**加以控制**。

**动态代理：**

![image-20210907153110383](DesignPatternofSixPrinciples/image-20210907153110383.png)

详解反射：https://blog.csdn.net/AlpinistWang/article/details/102852801

动态代理本质上与静态代理没有区别，它的好处是节省代码量。比如被代理类有 20 个方法，而我们只需要控制其中的两个方法，就可以**用动态代理通过方法名对被代理类进行动态的控制**，而如果用静态方法，我们就需要将另外的 18 个方法也写出来，非常繁琐。这就是动态代理的优势所在。

---

## 三、行为型模式 Behavioral Patterns

### 责任链模式：`Chain of responsibility`

> 处理职责相同，程度不同的对象，使其在一条链上传递





---

### 命令模式：`Command`

> 封装“方法调用”，将行为请求者和行为实现者解耦



---

### 解释器模式：`Interpreter`

> 定义自己的语法规则



---

### 迭代器模式：`Iterator`

> 定义 next() 方法和 hasNext() 方法，让外部类使用这两个方法来遍历列表，以达到隐藏列表内部细节的目的



---

### 中介者模式：`Mediator`

> 通过引入中介者，将网状耦合结构变成星型结构



---

### 备忘录模式：`Memento`

> 存储对象的状态，以便恢复



---

### 观察者模式：`Observer`

> 处理一对多的依赖关系，被观察的对象改变时，多个观察者都能收到通知



---

### 状态模式：State

> 关于多态的设计模式，每个状态类处理对象的一种状态



---

### 策略模式：Strategy

> 殊途同归，用多种方法做同一件事



### 模板方法模式：Template method

> 关于继承的设计模式，父类是子类的模板



---

### 访问者模式：Visitor

> 将数据的结构和对数据的操作分离



---

