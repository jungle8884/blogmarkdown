---
title: 软件设计师
categories:
  - 读书
  - 软件设计师
tags:
  - 原码, 反码, 补码, 移码
  - 进制转换
date: 2019-12-16 10:46:08
author: Jungle

---
# 原码 #
原码就是符号位加上真值的绝对值, 即用第一位表示符号, 其余位表示值.

> 参考：[为什么八位二进制数表示范围为-128~+127？—2018-08-01 - 简书 (jianshu.com)](https://www.jianshu.com/p/cf001200b1f9)

	[+1]原 = 0000 0001
	[-1]原 = 1000 0001
	
	对一个正数的原码取反加一，得到这个正数对应负数的补码。例如~6=-7，而且加一之后会多出一个八进制补码1000 0000，而这个补码就对应着原码1000 0000，数字位同时当做符号位即-128 。 
	所以根据以上我们可以理解为什么八位二进制数表示范围为-128~+127。
	
	八位二进制正数的补码范围是0000 0000 ~ 0111 1111 即0 ~ 127,负数的补码范围是正数的原码0000 0000 ~ 0111 1111 取反加一（也可以理解为负数1000 0000 ~ 1111 1111化为反码末尾再加一）。 所以得到 1 0000 0000 ~ 1000 0001,1000 0001作为补码，其原码是1111 1111（-127），依次往前推，可得到-1的补码为1111 1111，那么补码0000 0000的原码是1000 0000符号位同时也可以看做数字位即表示-128，这也解释了为什么127（0111 1111）+1（0000 0001）=-128（1000 0000）。

**对一个正数的原码取反加一，得到这个正数对应负数的补码。**

- 127：
  - [127]原: 0111 1111
  - [127]反: 1000 0000
  - [127]补:  1000 0001

- -128：
  - [-128]原: 1000 0000 【1既是符号位也是数字位。】
  - [-128]反: 1111 1111 【取反符号位不动】
  - [-128]补:  1 0000 0000

[127]原: 0111 1111 取反 即：[-128]原: 1000 0000 ，再加1 就是 [127]补:  1000 0001





----------
# 反码 #
- 正数的反码是其本身
- 负数的反码是在其原码的基础上, 符号位不变，其余各个位取反.

	[+1] = [00000001]原 = [00000001]反
	[-1] = [10000001]原 = [11111110]反

----------
# 补码 #
正数的补码就是其本身
负数的补码是在其原码的基础上, 符号位不变, 其余各位取反, 最后+1. (即在反码的基础上+1)

	[+1] = [00000001]原 = [00000001]反 = [00000001]补
	[-1] = [10000001]原 = [11111110]反 = [11111111]补

----------
# 移码 #
移码最简单了，不管正负数，只要将其补码的符号位取反即可。
例如：	 **X = -101011**

	[X]原= 10101011 
	[X]反=11010100
	[X]补=11010101
	[X]移=01010101

----------
# 进制转换 #

## 十进制小数转化为二进制小数的方法 ##

**对十进制小数乘以2得到的整数部分和小数部分，整数部分即是相应的二进制数码，再用2乘小数部分，结果再取整数部分，如此反复，直到小数部分为0或达到精度为止。第一次得到的为最高位，最后一次得到为最低位。**

	如计算+0.52的二进制：
	
	1、0.52*2=1.04 （取整得到1）
	
	2、0.04*2=0.08 （取整得到0）
	
	3、0.08*2=0.16 （取整得到0）
	
	4、0.16*2=0.32 （取整得到0）
	
	5、0.32*2=0.64 （取整得到0）
	
	6、0.64*2=1.28 （取整得到1）
	
	7、0.28*2=0.56 （取整得到0）
	……
	
	如果取机器字长为8情况下，则+0.52的二进制就是01000010；如果是32位的话，那就需要多算一会了

**对于小于-1的小数，需要拆分成整数部分和小数部分，整数采用除基数再倒取余数法。** 

	小数如上所述，以-6.25为例：
		
	a、整数部分为6：
	
	1、6/2=3　（取余数０）
	
	2、3/2=1　（取余数１）
	
	3、1/2=0  （取余数为1）
	
	那么整数6的二进制就是110
	
	b、小数部分为0.25
	
	1、0.25*2=0.5 （取整数0）
	
	2、0.5*2=1.0 （取整数1）
	
	所以小数部分0.25二进制就是01。（这里是不带符号位的6.25二进制表示）即：
	
		1110.01

----------
