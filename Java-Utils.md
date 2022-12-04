---
title: Java-Utils
categories:
  - 后端
  - Java
tags:
  - Utils
author:
  - Jungle
date: 2022-10-16 16:15:29
---

# Utils 的研究与应用

## Optional 的使用

- ofNullable
- ifPresent
- orElse
- filter
- map

![image-20221016161641894](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016161641894.png)

![image-20221016162634505](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016162634505.png)

## Arrays 的使用

- sort
- Arrays.toString()

![image-20221016163931481](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016163931481.png)

- binarySearch : 二分搜索的前提是排序好

![image-20221016164147814](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016164147814.png)

- stream() 流式操作 
- copyOf : 复制
- equals : 判断数组长度及每个元素的值是否一致
- fill : 填充
- setAll : 依次计算

![image-20221016164741973](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016164741973.png)

> 多维数组的 toString() 和 equals() 
>
> .deep...

- Arrays.toString() 只适用于一维数组

![image-20221016165219689](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016165219689.png)

- Arrays.deepToString(array) 适用于多维数组 

![image-20221016165418066](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016165418066.png)



- Arrays.asList() : 将数组转化为**固定长度**的List。【当作list，本质“还是数组”】

> 这是一个坑！

![image-20221016165951499](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016165951499.png)

> 如何解决这个问题？

把它当作参数，传入`new ArrayList<>(Arrays.asList(array))`

![image-20221016170549181](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016170549181.png)



> 转化未list之后的排序
>
> list.sort(Comparator.reverseOrder()); //逆序排序

![image-20221016170241992](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016170241992.png)



---



## Collections 的使用

> 一个专用于集合的工具类

![image-20221016170741367](C:\Users\Jungle\AppData\Roaming\Typora\typora-user-images\image-20221016170741367.png)

























