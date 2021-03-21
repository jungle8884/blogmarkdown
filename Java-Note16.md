---
title: Java-Note16
categories:
  - Java
tags:
  - 可变参数
date: 2020-11-05 18:57:21
author: Jungle

---
# 可变参数 #

- JDK1.5之后出现的新特性.
- 使用前提: 
	
	- 当参数列表数据类型已经确定, 但是参数的个数不确定, 就可以使用可变参数.
- 使用格式: 
	- 定义方法时使用 (如下)
	- 修饰符 返回值类型 方法名(数据类型 ... 变量名) {}
	
## 可变参数的原理:

**底层就是一个数组, 根据传递参数个数不同, 会创建不同长度的数组, 来存储这些参数.**

**传递的参数个数, 可以是 0个(即不传递参数), 或 1、2 ... 多个.**

```java
package demo;

public class DynamicParameters {
    public static void main(String[] args) {

        int sum = add(1, 2, 3, 4);
        System.out.println(sum);

    }

    /*
    * 计算 0~n 个整数的和
    * 参数的类型确定,
    * 但是参数的个数不确定.
    * --> 可以使用可变参数
    * */
    public static int add(int ... arr){

        int sum = 0;
        for (int i : arr) {
            sum += i;
            if (i != arr.length)
                System.out.print(i + " + ");
            else
                System.out.print(i + " = ");
        }

        return sum;
    }
}

// 输出:
1 + 2 + 3 + 4 = 10

Process finished with exit code 0
```

## 注意事项:

1. 一个方法的参数列表, 只能有一个可变参数;
2. 如果方法的参数有多个, 那么可变参数必须写在参数列表的末尾.