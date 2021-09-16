---
title: Calculate_date
categories:
  - 算法
  - 日期
tags:
  - 闰年
author:
  - Jungle
date: 2021-09-16 21:05:01
---

# Calculate date

> 输入一行字符串表示`当前日期`和`在当前日期上推移的秒数`, 比如: `2019, 12, 31, 23, 59, 59, 1`
>
> 即: 在`2019, 12, 31, 23, 59, 59` 加上 `1秒`
>
> 输出格式: `2020-01-01 00:00:00`



## 闰年计算公式

```java
if (year % 400 == 0 || year % 100 != 0 && year % 4 == 0) { ... }
```



## 代码实现

> 其中` rYear,  pYear`
>
> ```java
> /*
> *   闰年: 29 天
> *     1   2   3   4   5   6   7   8   9   10  11  12
> * 即: 31, 29, 31, 30, 31, 30, 31, 31, 30,  31, 30, 31
> * 	平年: 28 天
> * 即: 31, 28, 31, 30, 31, 30, 31, 31, 30,  31, 30, 31
> * */
> int[] rYear = new int[] {31, 29, 31, 30, 31, 30, 31, 31, 30,  31, 30, 31};
> int[] pYear = new int[] {31, 28, 31, 30, 31, 30, 31, 31, 30,  31, 30, 31};
> ```
>
> 是以空间换时间

```java
package com.OJ.SiKe;

import java.util.Scanner;

public class Main {
    // 在当前 日期上 加上 多少秒后, 现在是多久?
    public static void main(String[] args) {
        Scanner reader = new Scanner(System.in);
        String[] str = reader.nextLine().split(", ");
        // 2019, 12, 31, 23, 59, 59, 1
        int year = Integer.valueOf(str[0]);
        int month = Integer.valueOf(str[1]);
        int date = Integer.valueOf(str[2]); // day
        int hour = Integer.valueOf(str[3]);
        int minute = Integer.valueOf(str[4]);
        int second = Integer.valueOf(str[5]);

        int plus = Integer.valueOf(str[6]);
        int[] rYear = new int[] {31, 29, 31, 30, 31, 30, 31, 31, 30,  31, 30, 31};
        int[] pYear = new int[] {31, 28, 31, 30, 31, 30, 31, 31, 30,  31, 30, 31};

        // do-it:
        second += plus;
        if (second >= 60) {
            second %= 60;
            minute += 1;
            if (minute >= 60) {
                minute %= 60;
                hour += 1;
                if (hour >= 24) {
                    hour %= 24;
                    date += 1;

                    int tmpDate;
                    /*
                    *   闰年: 29 天
                    *     1   2   3   4   5   6   7   8   9   10  11  12
                    * 即: 31, 29, 31, 30, 31, 30, 31, 31, 30,  31, 30, 31
                    * 	平年: 28 天
                    * 即: 31, 28, 31, 30, 31, 30, 31, 31, 30,  31, 30, 31
                    * */
                    if (year % 400 == 0 ||
                            year % 100 != 0 && year % 4 == 0) {
                        tmpDate = rYear[month-1];
                    } else {
                        tmpDate = pYear[month-1];
                    }
                    if (date >= tmpDate) {
                        date %= tmpDate;
                        month += 1;
                        if (month >= 12) {
                            month %= 12;
                            year += 1;
                        }
                    }
                }
            }
        }

        // 2020-01-01 00:00:00
        StringBuilder sb = new StringBuilder();
        sb.append(year).append("-");

        String m = String.format("%02d", month);
        sb.append(m).append("-");

        String d = String.format("%02d", date);
        sb.append(d).append(" ");

        String h = String.format("%02d", hour);
        sb.append(h).append(":");

        String min = String.format("%02d", minute);
        sb.append(min).append(":");

        String s = String.format("%02d", second);
        sb.append(s);

        // output
        System.out.println(sb.toString());
    }
}
```

**注意输出格式:** 

- `String.format("%02d", month);`

- 长度为2, 不够补0