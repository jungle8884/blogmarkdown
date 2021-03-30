---
title: 工厂模式
categories:
  - 设计模式
  - 创建型模式
tags:
  - 简单工厂模式
  - 工厂模式
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

> 简单工厂模式的实现
>
> 实现了什么目的：
>
> - 让业务逻辑与界面逻辑分开，降低了耦合度
>
> - 只有分离开，才可以达到容易维护或扩展
>
> 不足之处：
>
> - 将来可能增加 开根运算，不仅需要增加相应的运算子类
> - 还需要在switch中增加分支

![image-20210330214708757](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210330214708757.png)

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



# 工厂模式









# 抽象工厂模式