---
title: Week = (Day + 2*Month + 3*(Month+1)/5 + Year + Year/4 - Year/100 + Year/400) % 7
categories:
  - 算法
  - 日期
tags:
  - 蔡勒公式
  - OJ
author:
  - Jungle
date: 2021-06-27 
---

# [蔡勒（Zeller）公式](https://www.cnblogs.com/tgycoder/p/4960487.html)

> Zeller公式是一个计算星期的公式, 随便给一个日期, 就能用这个公式计算出是星期几.

## 公式

```java
/*  蔡勒（Zeller）公式:
*   Week=(Day + 2*Month + 3*(Month+1）/5 + Year + Year/4 - Year/100 + Year/400) % 7
*   0 是 星期1;
*   1-星期2，2-星期3，3-星期4，4-星期5，5-星期6，6-星期日;
 * */
public static int Week(int year, int month, int day) {
    int result = (day + 2*month + 3*(month+1)/5
            + year + year/4 - year/100 + year/400) % 7;
    return result;
}
```

## 注意

- week: 代表星期
  - week = result % 7 
  - 0 是 星期1; 
  - 1-星期2，2-星期3，3-星期4，4-星期5，5-星期6，6-星期日;

- 该公式中要把1月和2月分别当成上一年的13月和14月处理
  - 比如2003年1月1日要看作2002年的13月1日来计算
- 闰年: 
  1. `能被4整除, 但不能被100整除` 是闰年, 如 2008年是闰年
  2. `能被400整除` 是闰年, 如2000年也是闰年

## 例题

> 输入两个年份, a 和b;
>
> 计算从yearStart的1月到yearEnd的12月, 每月的第一天是星期一有几天.
>
> `Week=(Day + 2*Month + 3*(Month+1）/5 + Year + Year/4 - Year/100 + Year/400) % 7`
>
> 分析: 
>
> - `yearStart的1月`开始, 即 yearStart-01-01 : 公式中 Day = 1
> - 到`yearEnd的12月`结束, 即 yearEnd-12-31: 循环 for (int i = yearStart; i <= yearEnd; i++) 次

```java
import java.util.Scanner;

/*
* 蔡勒（Zeller）公式:
* Week=(Day + 2*Month + 3*(Month+1）/5 + Year + Year/4 - Year/100 + Year/400) % 7
* */
public class OJ {
    public static void main(String[] args) {
        Scanner reader = new Scanner(System.in);
        int yearStart = reader.nextInt();
        int yearEnd = reader.nextInt();
        int count = 0;

        // yearStart 的 1月 到 yearEnd 的 12 月 有多少个月份的第一天是星期一
        for (int i = yearStart; i <= yearEnd; i++) {
            for (int j = 1; j <= 12; j++) {
				// y 为年份, m 为月份
                int y = i, m = j; 
                // 把1月和2月分别当成上一年的13月和14月处理
                if (j == 1 || j == 2) {
                    m += 12;
                    y--;
                }
                // Day = 1, 计算出星期数
                int result = (1 + 2*m + 3*(m+1)/5 + y + y/4 - y/100 + y/400) % 7;
				// 0 代表星期一
                if (result == 0) {
                    count++;
                }
            }
        }

        // 输出有多少个星期一
        System.out.println(count);
    }
}
```

