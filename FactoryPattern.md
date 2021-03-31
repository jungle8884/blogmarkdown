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