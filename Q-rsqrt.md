---
title: Q_rsqrt
categories:
  - 算法
  - 数学
tags:
  - 快速平方根
author:
  - Jungle
date: 2023-11-01 08:52:08
---

# 平方根

## 牛顿迭代法

> 数学推导过程晚点补上
>
> **Xn+1 = Xn - f(X1)/f`(X1)** 
>
> f(x) = x^2 - 2 ；当f(x) = 0时，x=根号2

```py
# 牛顿迭代法计算平方跟 
def newton_sqrt_2(precision=1e-7):
    x = 1.0 #初始值 // x = 1.414  # 经验值
    while True:
        next_x = 0.5 * (x + 2/x)
        if abs(next_x - x) < precision:
            return next_x;
        x = next_x

sqrt_2 = newton_sqrt_2()
print(f"2的平方跟的近似值为:{sqrt_2:.7f}")
```

## 快速平方根

> 推导过程：

```c
#include <stdio.h>

// 快速反平方根算法: 它接受一个float类型的参数number，该参数是要计算倒数平方根的数值。
float Q_rsqrt(float number) {
    long i; // long i;：声明一个long类型的变量i，用于进行位操作。
	float x2, y; // 声明两个float类型的变量x2和y，它们用于在算法中存储中间值。
	const float threehalfs = 1.5F; // 定义一个常量threehalfs并初始化为1.5，它在后面的计算中使用。
	
	x2 = number * 0.5F; // 将x2设置为number的0.5倍
	y = number; // 将y设置为number的值。
	i = * ( long * ) & y; // 这一行进行了一些位操作，它将y的内存表示转换为long类型的整数i。
	i = 0x5f3759df - (i>>1); // 这一行将i的值右移1位，然后从0x5f3759df中减去，这也是快速反平方根算法的关键步骤。这个值0x5f3759df是一个经验值，经过研究得到，用于改进近似性能。
	
	y = * (float *) &i; // 这一行将i的内存表示转回为float类型的值y。
	y = y * ( threehalfs - (x2 * y * y) ); // 这一行使用了前面计算的y、x2和threehalfs来进行迭代计算，以逼近倒数平方根的近似值。
	
	return y;
}

int main() {
    float number = 2.0; // 要计算平方根的数字
    float result = Q_rsqrt(number);

    printf("Q_rsqrt(%.2f) = %.7f\n", number, result);
    return 0;
}
```

