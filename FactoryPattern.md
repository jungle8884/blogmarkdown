---
title: 工厂模式
categories:
  - 设计模式
tags:
  - 创建型模式
  - 简单工厂模式
  - 工厂方法模式
  - 抽象工厂模式
date: 2021-3-30
author: jungle
---

> 工厂：用一个单独的类来做这个创造实例的过程。
>
> 任务：实现一个计算器。
>
> 说明：本文章参考自大话设计模式(C# 实现)，本人用 Java 实现而已。

# 简单工厂模式

## 代码设计

![image-20210330214708757](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210330214708757.png)

> 运算类（基类）
>

```java
package FactoryPattern.SimpleFactoryPattern;

public class Operation {
    private double numberA = 0;
    private double numberB = 0;

    public double getNumberA() {
        return this.numberA;
    }

    public void setNumberA(double value) {
        this.numberA = value;
    }

    public double getNumberB() {
        return this.numberB;
    }

    public void setNumberB(double value) {
        this.numberB = value;
    }

    public double GetResult() {
        double result = 0;
        
        return result;
    }
}
```

> 加法类

```java
package FactoryPattern.SimpleFactoryPattern;

public class OperationAdd extends Operation{
    @Override
    public double GetResult() {
        double result = 0;
        result = getNumberA() + getNumberB();

        return result;
    }
}
```

> 减法类

```java
package FactoryPattern.SimpleFactoryPattern;

public class OperationSub extends Operation {
    @Override
    public double GetResult() {
        double result = 0;
        result = getNumberA() - getNumberB();

        return result;
    }
}
```

> 乘法类

```java
package FactoryPattern.SimpleFactoryPattern;

public class OperationMul extends Operation {
    @Override
    public double GetResult() {
        double result = 0;
        result = getNumberA() * getNumberB();

        return result;
    }
}
```

> 除法类

```java
package FactoryPattern.SimpleFactoryPattern;

public class OperationDiv extends Operation {
    @Override
    public double GetResult() {
        double result = 0;
        result = getNumberA() / getNumberB();

        return result;
    }
}
```

## 简单工厂模式的实现

> 实现了什么目的：
>
> - 让业务逻辑与界面逻辑分开，降低了耦合度
> - 只有分离开，才可以达到容易维护或扩展
>
> 优点：
>
> - 在于工厂类中包含了必要的逻辑判断，根据客户端的选择条件动态化实例相关的类
> - 对于客户端来说，去除了与具体产品的依赖
>
> 不足之处：
>
> - 将来可能增加 开根运算，不仅需要增加相应的运算子类，还需要在switch中增加分支
> - 这就等于说，我们不但对扩展开放了，对修改也开放了，这样就违背了 **开放-封闭原则**

```java
package FactoryPattern.SimpleFactoryPattern;

public class OperationSimpleFactory {
    public static Operation createOperate(String operate) {
        Operation oper = null;
        // 界面逻辑
        switch (operate) {
            case "+" :
            oper = new OperationAdd(); //业务逻辑
            break;
            case "-" :
            oper = new OperationSub();
            break;
            case "*" : 
            oper = new OperationSub();
            break;
            case "/" :
            oper = new OperationDiv();
            break;
        }
        return oper;
    }
}
```

> 测试

```java
package FactoryPattern.SimpleFactoryPattern;

public class Test {
    public static void main(String[] args) {
        Operation oper = null;
        oper = OperationSimpleFactory.createOperate("+"); //通过多态，返回父类的方式实现了计算器的结果
        oper.setNumberA(1);
        oper.setNumberB(2);
        System.out.println(oper.GetResult());
    }
}
```

> 输出：
>
> 3.0



# 工厂方法模式

![image-20210331185044469](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210331185044469.png)

## 代码设计

> 工厂接口

```java
package FactoryPattern.Factory;

import FactoryPattern.*;

public interface IFactory {
    Operation createOperation();
}
```

> 加法类工厂

```java
package FactoryPattern.Factory;

import FactoryPattern.*;

public class AddFactory implements IFactory {
    @Override
    public Operation createOperation() {
        return new OperationAdd();
    }
}
```

> 减法类工厂

```java
package FactoryPattern.Factory;

import FactoryPattern.*;

public class SubFactory implements IFactory {
    @Override
    public Operation createOperation() {
        return new OperationSub();
    }
}
```

> 乘法类工厂

```java
package FactoryPattern.Factory;

import FactoryPattern.*;

public class MulFactory implements IFactory {
    @Override
    public Operation createOperation() {
        return new OperationMul();
    }
}
```

> 除法类

```java
package FactoryPattern.Factory;

import FactoryPattern.*;

public class DivFactory implements IFactory {
    @Override
    public Operation createOperation() {
        return new OperationDiv();
    }
}
```

## 工厂方法模式的实现

> 工厂方法模式：定义一个用于创建对象的接口，让子类决定实例化哪一个类，工厂方法使一个类的实例化延迟到其子类。
>
> - 依赖倒置原则[Dependence Inversion Principle]：**高层模块不应该依赖低层模块，两者都应该依赖其抽象**；抽象不应该依赖细节，**细节应该依赖抽象**.
> - 开闭原则[Open Closed Principle，OCP]：**对扩展开放, 对修改关闭**.
>
> 实现了什么目的：
>
> - 将（简单）工厂类与分支解耦，根据 **依赖倒置原则**，我们把工厂类抽象出一个接口，这个接口只有一个创建抽象产品的工厂方法；所有的要生产具体类的工厂，就去实现这个接口。
> - 将一个简单工厂模式的工厂类（OperationSimpleFactory），扩展成了一个工厂抽象类接口（IFactory）和多个具体生成对象的工厂（AddFactory ...）；
> - 当需要增加开根号的运算时，只需要增加此功能的运算类（OperationSqrt）和相应的工厂类(SqrtFactory implements IFactory)即可，不会违反 **开放-封闭** 原则。

![image-20210331184928077](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210331184928077.png)

```java
package FactoryPattern.Factory;

import FactoryPattern.Operation;

public class Test {
    public static void main(String[] args) {
        IFactory operFactory = new AddFactory();
        Operation oper = operFactory.createOperation();
        oper.setNumberA(1);
        oper.setNumberB(2);
        System.out.println(oper.GetResult());
    }
}
```



# 抽象工厂模式

> 回顾工厂方法模式

![image-20210401192456234](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210401192456234.png)

> User : （数据库访问类）用户类

```java
package FactoryPattern.AbstractFactory;

public class User {
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    private int id;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    private String name;
    
}
```

> IUser 接口：用于客户端访问，解除与具体数据库访问的耦合。

```java
package FactoryPattern.AbstractFactory;

interface IUser {
    void Inser(User user);
    User GetUser(int id);
}
```

> MySqlUser：用于访问MySql的User （替换了结构图中的AccessUser）

```java
package FactoryPattern.AbstractFactory;

public class MySqlUser implements IUser {

    @Override
    public void Inser(User user) {
        System.out.println("在Mysql中给User表增加一条记录");
    }

    @Override
    public User GetUser(int id) {
        System.out.println("在Mysql中根据ID得到User表一条记录");
        return null;
    }
    
}
```

> SqlServerUser：用于访问SqlServer的User

```java
package FactoryPattern.AbstractFactory;

public class SqlServerUser implements IUser {
    @Override
    public void Inser(User user) {
        System.out.println("在SqlServer中给USer表增加一条记录");
    }

    @Override
    public User GetUser(int id) {
        System.out.println("在SqlServer中根据ID得到一条记录");
        return null;
    }
}
```

> IFactory接口：定义一个创建 访问User表对象 的 抽象工厂 的 接口

```java
package FactoryPattern.AbstractFactory;

public interface IFactory {
    IUser createUser();
}
```

> MySqlFactory类：实现IFactory接口，实例化MySqlUser（替换了结构图中的AccessFactory）

```java
package FactoryPattern.AbstractFactory;

public class MySqlFactory implements IFactory {

    @Override
    public IUser createUser() {
        return new MySqlUser();
    }
    
}
```

> SqlServerFactory类：实现IFactory接口，实例化SqlServerUser

```java
package FactoryPattern.AbstractFactory;

public class SqlServerFactory implements IFactory {

    @Override
    public IUser createUser() {
        return new SqlServerUser();
    }
    
}
```

> Test：客户端代码

```java
package FactoryPattern.AbstractFactory;

public class Test {
    public static void main(String[] args) {
        User user = new User();

        // 若需要对SqlServer数据库操作，只需要将本局改成：IFactory factory = new SqlServerFactory();
        IFactory factory = new MySqlFactory();
        IUser usr = factory.createUser();

        usr.Inser(user);
        usr.GetUser(1);
    }
}
```

> **问题来了**：只有一个User类和User操作类的时候，只需要工厂方法模式即可；

> 只增加一张部门表如下：
>
> ![image-20210401195912460](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210401195912460.png)

> Department : 数据库对应的表

```java
package FactoryPattern.AbstractFactory;

public class Department {
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    private int id;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    private String name;
}
```

> IDepartment ：定义操作

```java
package FactoryPattern.AbstractFactory;

public interface IDepartment {
    void Inser(Department dept);
    User GetDepartment(int id);
}
```

> MySqlDepartment：实现对MySql的操作

```java
package FactoryPattern.AbstractFactory;

public class MySqlDepartment implements IDepartment {

    @Override
    public void Inser(Department dept) {
        // Department
        System.out.println("在Mysql中给Department表增加一条记录");
    }

    @Override
    public User GetDepartment(int id) {
        // Department
        System.out.println("在Mysql中根据ID得到Department表一条记录");
        return null;
    }
    
}
```

> SqlServerDepartment：实习对SqlServer的操作

```java
package FactoryPattern.AbstractFactory;

public class SqlServerDepartment implements IDepartment {

    @Override
    public void Inser(Department dept) {
        // Department
        System.out.println("在SqlServer中给Department表增加一条记录");
    }

    @Override
    public User GetDepartment(int id) {
        // Department
        System.out.println("在SqlServer中根据ID得到Department表一条记录");
        return null;
    }
    
}
```

> 对IFactory的扩展

```java
package FactoryPattern.AbstractFactory;

public interface IFactory {
    IUser createUser();
    IDepartment createDepartment();
}
```

> 对 MySqlFactory 的扩展

```java
package FactoryPattern.AbstractFactory;

public class MySqlFactory implements IFactory {

    @Override
    public IUser createUser() {
        return new MySqlUser();
    }

    @Override
    public IDepartment createDepartment() {
        // Department
        return new MySqlDepartment();
    }
    
}
```

> 对 SqlServerFactory 的扩展

```java
package FactoryPattern.AbstractFactory;

public class SqlServerFactory implements IFactory {

    @Override
    public IUser createUser() {
        return new SqlServerUser();
    }

    @Override
    public IDepartment createDepartment() {
        // Department
        return new SqlServerDepartment();
    }
    
}
```

> 客户端代码 ：Test类

```java
package FactoryPattern.AbstractFactory;

public class Test {
    public static void main(String[] args) {
        User user = new User();
        Department dept = new Department();

        // 若需要对SqlServer数据库操作，只需要将本局改成：IFactory factory = new SqlServerFactory();
        IFactory factory = new MySqlFactory();
        
        IUser usr = factory.createUser();       
        usr.Inser(user);
        usr.GetUser(1);
        
		IDepartment dpt = factory.createDepartment();
        dpt.Inser(dept);
        dpt.GetDepartment(1);
    }
}
```

> 运行结果：
>
> 在Mysql中给User表增加一条记录
> 在Mysql中根据ID得到User表一条记录      
> 在Mysql中给Department表增加一条记录    
> 在Mysql中根据ID得到Department表一条记录



**以上是两张表的操作**。



> 将上面的内容抽象出来，就是抽象工厂模式。
>
> - AbstractProductA 对应 IUser
>   - ProductA 1 对应  SqlServerUser
>   - ProductA 2 对应 MySqlUser
> - AbstractProductB 对应 IDepartment
>   - ProductA 1 对应  SqlServerDepartment
>   - ProductA B 对应  MySqlDepartment
> - AbstractFactory 对应 IFactory
>   - ConcreateFactory 1 对应 SqlServerFactory
>   - ConcreateFactory 2 对应 MySqlFactory
>
> 通常是在运行时刻再创建一个ConcreteFactory类的实例（比如 ：IFactory factory = new MySqlFactory() ），这个具体的工厂再创建具有特定实现的产品对象（比如 IUser usr = factory.createUser() ），
>
> 也就是说，为创建不同的产品对象，客户端应使用不同的具体工厂。



> 抽象工厂模式：提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
>
> - 优点：
>   - 易于交换产品系列，由于具体工厂类，在一个应用中只需要在初始化的时候出现一次（比如：IFactory factory = new MySqlFactory() ），这就使得改变一个应用的具体工厂变得非常容易，只需要改变具体工厂即可使用不同的产品配置；
>   - 让具体的创建实例过程与客户端分离（比如：IUser usr = factory.createUser() ），客户端是通过它们的抽象接口操纵实例（ 比如：usr.Inser(user) ），产品的具体类名也被具体工厂（比如：MySqlFactory）的实现分离，不会出现在客户代码中。
> - 缺点:
>   - 每增加一张项目表（比如Project表），就需要增加三个类：IProject，SqlServerProject，MySqlProject 以及扩展（修改） IFactory，SqlServerFactory，MySqlFactory。
>   - 很明显，扩展的时候会对IFactory，SqlServerFactory，MySqlFactory进行修改，违反了 **开发-封闭** 原则

![image-20210401193738506](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210401193738506.png)

> 为创建不同的产品对象，客户端应使用不同的具体工厂。

> 用反射+抽象工厂的数据访问程序...

> 任务：用Java实现MySql等多种数据库的增删查改...







