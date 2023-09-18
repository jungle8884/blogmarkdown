---
title: JobInterview
categories:
  - 算法
  - 面试题
tags:
  - N的阶乘
author:
  - Jungle
date: 2023-09-18 21:55:19
---

# 面试题

## N的阶乘

> 要求：
>
> - 不要用递归 -> 【可以用循环】
> - int, long 会溢出 【数组保存】

```java
/**
 * 计算 n 的阶乘
 * @author Jungle 
 * @version 2023/09/18 17:53
**/
public class calcN {
    public static void main(String[] args) {
//        factorial(3);
//        factorial(10);
        factorial(100);
    }

    public static void factorial(int n) {
        //创建保存结果的数组array
        int[] array = new int[n*n];

        // 参数初始化
        array[0] = 1;
        //位数 bit【几位数】
        int bit = 1;
        //进位 carry
        int carry = 0;

        // eg: 6! = 6 * 5 * 4 * 3 * 2 * 1 = 5! * 6 = 120 * 6
        for(int i = 2;i <= n; i++) {
            // 计算每一轮结果：从2的阶乘开始计算，算到n
            for(int j = 0; j < bit; j++) {
                array[j] = array[j] * i;
            }
            // 1*6=6; 2*6=12; 0*6=0 【i==6时: bit-1 == 2-1 == 1, 进位一个】
            for(int j = 0; j < bit-1; j++) {
                //进位数 【2*6=12 -> 1】
                carry = array[j]/10;
                //每个数组元素都只留下个位数【2*6=12 -> 2】
                array[j] %= 10;
                //上一位加上进位的结果
                array[j+1] += carry;
            }
            //如果最高位大于等于10，那么需要再进一位
            if(array[bit-1] >= 10) {
                int tempCarry = array[bit-1] / 10;
                array[bit-1] %= 10;
                //一直进位直到进位数为0
                while(tempCarry != 0) {
                    array[bit] = tempCarry % 10;
                    tempCarry /= 10;
                    bit++;
                }
            }
        }

        // 6! 的计算结果的表示：0 2 7 【逆序的】
        System.out.println("计算结果: ");
        for(int m = bit - 1; m >= 0; m--) {
            System.out.print(array[m]);
        }
        System.out.println("\n几位数: " + bit);
    }
}
```

