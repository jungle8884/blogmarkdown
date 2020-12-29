---
title: 算法笔记-入门
categories:
  - 算法笔记
tags:
  - 算法
  - C++
  - 入门模拟
  - 数学问题
date: 2020-09-28 16:36:16
author: Jungle

---
# PAT-B1001-害死人不偿命的(3n+1)猜想 #

## 描述 ##
**卡拉兹猜想:** 

	对任何一个自然数n,
	 	如果它是偶数, 那么把它砍掉一半; 
		如果它是奇数, 那么把 (3n+1) 砍掉一半.
	这样一直反复砍下去, 最后一定在某一步得到n=1.
	
		卡拉兹在1905年的世界数学家大会上公布了这个猜想, 
	传说当时耶鲁大学师生齐动员, 拼命想证明这个貌似很荒唐...
	
		此处并非要证明卡拉兹猜想, 
	而是对给定的任一不超过1000的正整数n, 
	简单地数一下, 需要多少步才能得到n=1?
	 
- 输入格式: 每个测试输入包含一个测试用例, 即给出自然数n的值. 
- 输出格式: 输出从n计算到1需要的步数. 

## 代码分析 ##
	# include <cstdio> 
	
	int main(){
		int n, step = 0;
		scanf("%d", &n);
		while(n != 1){
			if(n % 2 == 0) n = n / 2;	
			else n = (3 * n + 1) / 2;
			step++;
		}
		printf("%d\n", step);
		return 0;
	} 

## 运行结果 ##
	3
	5
	
	--------------------------------
	Process exited with return value 0
	Press any key to continue . . .

----------
# PAT-B1032-挖掘机技术哪家强 #

## 描述 ##
	为了用事实说明挖掘机技术到底哪家强, PAT组织了一场挖掘机技能大赛.
	请根据比赛结果统计出技术最强的那个学校.

- 输入格式: 
	- 在第1行给出不超过10^5的正整数N, 即参赛人数. 
	- 随后N行, 
		- 每行给出一位参赛者的信息和成绩, 
		- 包括其所代表的学校的编号(从1开始连续编号)
		- 及其	比赛成绩(百分制), 中间以空格分隔. 
		- 例如: 1 100
- 输出格式: 在一行中给出总得分最高的学校的编号及其总分, 中间以空格分隔. 

- 输入样例:

		6
		3 65
		2 80
		1 100
		2 70
		3 40
		3 0

- 输出示例:

		2 150 

## 代码分析 ##
	#include <cstdio> 
	
	const int maxn = 100010;
	int school[maxn] = {0}; //记录每个学校的总分
	int main(){
		int n, schID, score;
		scanf("%d", &n);
		for(int i = 0; i < n; i++){
			scanf("%d%d", &schID, &score); //学校ID, 分数 
			school[schID] += score; //学校schID的总分增加score 
		}
		int k = 1, MAX = -1; //最高总分的学校ID以及其总分
		for(int i = 1; i <= n; i++){ //从所有学校中选出总分最高的一个 
			if(school[i] > MAX){
				MAX = school[i];
				k = i; 
			} 
		} 
		
		printf("%d %d\n", k, MAX); //输出最高总分的学校ID及其总分 
		return 0;
	} 

## 运行结果 ##
	6
	3 65
	2 80
	1 100
	2 70
	3 40
	3 0
	2 150
	
	--------------------------------
	Process exited with return value 0
	Press any key to continue . . .

----------
# codeup-查找元素 #

## 描述 ##
	输入一个数 n (1 <= n <= 200), 
	然后输入 n 个数值不相同的数, 
	再输入一个值 X, 
	输出这个值在这个数组中的下标 (从 0 开始, 若不在数组中则输出 -1) 

## 代码分析 ##
	#include <cstdio>
	
	const int maxn = 210;
	int a[maxn]; //存放n个数
	
	int main(){
		int n, x;
		
		scanf("%d", &n);
		for(int i = 0; i < n; i++){ //输入n个数
			scanf("%d", &a[i]);  
		}
	
		
		scanf("%d", &x); //输入欲查询的数
		int k; //下标 
		
		for(int k = 0; k < n; k++){ //遍历数组 
			if(a[k] == x){ //如果找到了x 
				printf("%d\n", k); //输出对应的下标 
				break; //退出查找 
			}
		} 
		
		if(k == n){ //如果没有找到 
			printf("-1\n"); //输出 -1 
		} 
		
		return 0; 
	} 

## 运行结果 ##
	4
	1 2 3 4
	3
	2
	
	--------------------------------
	Process exited with return value 0
	Press any key to continue . . .


## 注意事项:  ##

- EOF，为End Of File的缩写，通常在文本的最后存在此字符表示资料结束。

	在微软的DOS和Windows中，读取数据时终端不会产生EOF。
	此时，应用程序知道数据源是一个终端（或者其它“字符设备”），
	并将一个已知的保留的字符或序列解释为文件结束的指明；最
	普遍地说，它是ASCII码中的替换字符（Control-Z，代码26）。		
	在终端(黑框)中手动输入时，
	系统并不知道什么时候到达了所谓的“文件末尾”，
	因此需要用<Ctrl + z>组合键然后按 Enter 键的方式来告诉系统已经到了EOF，
	这样系统才会结束while.

- 程序中的参考代码片段如下, 其被我替换成了文中的代码

			while(scanf("%d", &n) != EOF){
				for(int i = 0; i < n; i++){
					scanf("%d", &a[i]); //输入n个数 
				}
			}
			
----------
# B1036-跟奥巴马一起编程 #

## 描述 ##
	美国总统奥巴马不仅呼吁所有人都学习编程，甚至以身作则编写代码，成为美国历史上首位编写计算机代码的总统。
	2014 年底，为庆祝“计算机科学教育周”正式启动，奥巴马编写了很简单的计算机代码：在屏幕上画一个正方形。
	现在你也跟他一起画吧！

- 输入格式：

		输入在一行中给出正方形边长 N（3≤N≤20）和组成正方形边的某种字符 C，间隔一个空格。

- 输出格式：

		输出由给定字符 C 画出的正方形。
		但是注意到行间距比列间距大，所以为了让结果看上去更像正方形，我们输出的行数实际上是列数的 50%（四舍五入取整）。

- 输入样例：

		10 a

- 输出样例：

		aaaaaaaaaa
		a        a
		a        a
		a        a
		aaaaaaaaaa

## 代码分析 ##
	# include<cstdio>
	
	int main(){
		int row, col; //行, 列 
		char c;
		
		scanf("%d %c", &col, &c); //输入列数, 欲使用的字符 
		
		if(col % 2 == 1){  //对行数进行四舍五入 
			row = col / 2 + 1;
		}else{
			row = col / 2;
		}
		
		// 第 1 行
		for(int i = 0; i < col; i++){
			printf("%c", c); //输出 col 个字符 
		} 
		printf("\n");
		
		// 第 2 ~ row-1 行
		for(int i = 1; i < row-1; i++){
			printf("%c", c); //每行的第一个字符 c 
			for(int j = 0; j < col-2; j++){
				printf(" "); //输出 col-2 个空格 
			}
			printf("%c\n", c); //每行的最后一个字符 c 
		}
		
		// 第 row-1 行
		for(int i = 0; i < col; i++){
			printf("%c", c);
		} 
		
		return 0;
	}

## 运行结果 ##
		10 *
		**********
		*        *
		*        *
		*        *
		**********
	
	--------------------------------
	Process exited with return value 0
	Press any key to continue . . .

## 注意事项: ##
	整数除以2进行四舍五入的操作可以通过判断它是否是奇数来解决, 
	以避免浮点数的介入.
	
	文中代码如下:
		说输出的行数实际上是列数的50%(四舍五入取整)

		if(col % 2 == 1){  //对行数进行四舍五入 
			row = col / 2 + 1;
		}
		else{
			row = col / 2;
		}

----------
# codeup 1928-日期差值 #

## 描述 ##
 有两个日期, 求两个日期之间的天数, 如果两个日期是连续的, 则规定它们之间的天数为两天.

- 输入格式: 

		有多组数据, 每组数据有两行, 分别表示两个日期, 形式为 YYYYYMMDD.

- 输出格式:

		每组数据输出一行, 即日期差值.

- 样例输入:

		20130101
		20130105

- 样例输出:

		5

## 代码分析 ##

思路: 

- 不妨假设第一个日期早于第二个日期(否则交换即可).
- 令日期不断加1一天, 直到第一个日期等于第二个日期为止.

		#include <cstdio>
		
		int month[13][2] = { 
			//平年和闰年每个月的天数 
			{0, 0},
			{31, 31}, //一月大 
			{28, 29}, //二月小
			{31, 31}, //三月大 
			{30, 30}, //四月小 
			{31, 31}, //五月大
			{30, 30}, //六月小
			{31, 31}, 
			{31, 31}, //七月大, 八月大
			{30, 30}, //九月小
			{31, 31}, //十月大
			{30, 30}, //十一月小
			{31, 31}  //十二月大 
		};
		
		//判断是否是闰年 
		bool isLeap(int year){
			return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0); 
		} 
		
		int main(){
			int time1, y1, m1, d1;
			int time2, y2, m2, d2;
			
			while(scanf("%d%d", &time1, &time2) != EOF){
				if(time1 > time2){
					int temp = time1;
					time1 = time2;
					time2 = temp;
				}
				
				y1 = time1 / 10000, m1 = time1 % 10000 / 100, d1 = time1 % 100;
				y2 = time2 / 10000, m2 = time2 % 10000 / 100, d2 = time2 % 100;
				
				int ans = 1; //记录结果---日期差值 
				//第一个日期没有达到第二个日期时进行循环
				//即 !((y1 == y2) && (m1 == m2) && (d1 == d2)) 
				while(y1 < y2 || m1 < m2 || d1 < d2){
					d1++; //天数加1 
					if(d1 == month[m1][isLeap(y1)]+1){ //是否满当月的天数 
						m1++; //日期变为下个月的1号.
						d1 = 1; 
					}
					if(m1 == 13){ //月份是否满十二个月 
						y1++; // 日期变为下一年的一月 
						m1 = 1; 
					}
					ans++; //累计 
				}
				printf("%d\n", ans); //输出结果 
			}
			
			return 0;
		}

## 运行结果: ##

	20130101
	20130105
	5

----------
# 1022 D进制的A+B  #

## 描述 ##
	输入两个非负 10 进制整数 A 和 B ( ≤ 2​的30​​次方 −1)，输出 A+B 的 D (1<D≤10) 进制数。

## 输入格式： ##
	输入在一行中依次给出 3 个整数 A、B 和 D。

## 输出格式： ##
	输出 A+B 的 D 进制数。

## 输入样例： ##
	123 456 8
## 输出样例： ##
	1103
## 思路 ##
	先计算 A+B(此时为十进制), 然后把结果转换为 D进制, 而十进制转换为 D进制的过程可以直接进行 "除基取余法".
## 代码描述 ##
	#include <cstdio>
	#include <cstdlib>
	
	int index = 0; //p进制数x 用十进制数组表示的数位数. 
	
	// 将 p进制数x 转换为 十进制数y. 
	int P2DEC(int x, int p){ //x 代表 p(1 <= p <= 10)进制数, 用十进制的数字来表示. 
		int y = 0, product = 1;
		while(x != 0){
			y += (x % 10) * product; //X % 10 是为了每次获取x的个位数 
			x /= 10; //去掉x的个位
			product *= p; 
		}
		
		return y;
	}
	
	// 将 十进制数y 转化为 p(1 <= p <= 10)进制数x, x用十进制的数字来表示.
	void DEC2P(int y, int p, int* x){
		//数组x 存放 p进制数y 的每一位, index为位数.
		do{ //除基取余 
			x[index++] = y % p;
			y /= p; 
		} while(y != 0);	
		
		return;
	}
	
	int main(){
		int a, b, d;
		scanf("%d%d%d", &a, &b, &d);
		
		int x_dec, x = 0;
		
		int sum = a + b; 
		int ans[31], num = 0; //ans存放D进制的每一位
		
		DEC2P(sum, d, ans); //进制转换 
		
		for(int i = index - 1; i >= 0; i--){ //从高位到低位进行输出 
			printf("%d", ans[i]);
			
		/*
			//将其从数组形式转换为数字形式 
			x_dec = 1;
			for(int j = 0; j < i; j++){ //先获取当前位数的权值 
				x_dec *= 10;
			}	
			x += ans[i] * x_dec;
		*/
		}
	/*
		printf("\n%d", x); //x 表示 ans 的数字形式 
		printf("\n%d", P2DEC(x, d)); //将x从d进制转换为十进制并输出 
	*/
		
		return 0;
	} 

## 输出结果: ##
	123 456 8
	1103
	--------------------------------
	Process exited with return value 0
	Press any key to continue . . .

----------
# codeup-5901 #

## 描述 ##
	读入一串字符, 判断是否是 "回文串".
	"回文串" 是一个正读和反读都一样的字符串, 
	比如 "level" 和 "noon" 就是回文串.
## 输入格式 ##
	一行字符串, 长度不超过255.
## 输出格式 ##
	如果是 "回文串", 输出 "YES", 否则输出 "NO".
## 样例输出 ##
	12321
## 样例输出 ##
	YES
## 思路 ##
	假设字符串str的下标从0开始, 由于 "回文串" 是正读和反读都一样的字符串, 因此只需要遍历字符串的前一半(注意: 不需要取到 i == len / 2), 如果出现字符 str[i] 不等于其对称位置 str[len-1-i], 那么说明这个字符串 不是 "回文串"; 如果出现字符 str[i] 等于其对称位置 str[len-1-i], 那么说明这个字符串 是 "回文串".

## 代码分析 ##
	#include <cstdio>
	#include <cstring>
	
	const int maxn = 256;
	
	//判断字符串str是否是 "回文串".
	bool judge(char str[]){
		int len = strlen(str);
		for(int i = 0; i < len / 2; i++){ //i 枚举字符串的前一半 
			if(str[i] != str[len-1-i]){ //如果对称位置不同 
				return false; 
			} 
		}
		
		return true;
	} 
	
	int main(){
		char str[maxn];
		/*
		gets()函数从STDIN(标准输入)读取字符并把它们加载到str(字符串)里,	直到遇到新行(\n)或到达EOF. 
		新行字符翻译为一个null中断符. 
		gets()的返回值是读入的字符串,如果错误返回NULL. 
		*/
		while(gets(str)){ //输入字符串 
			bool flag = judge(str); //判断字符串 str 是否是 "回文串".
			if(flag == true){ //"回文串"
				printf("YES\n");
			} else{ //不是 "回文串" 
				printf("NO\n"); 
			} 
		}
		
		return 0;
	}

----------
# B1009-说反话 #

## 描述 ##
	给定一句英语，要求你编写程序，将句中所有单词的顺序颠倒输出。
## 输入格式： ##
	测试输入包含一个测试用例，在一行内给出总长度不超过 80 的字符串。
	字符串由若干单词和若干空格组成，
	其中单词是由英文字母（大小写有区分）组成的字符串，
	单词之间用 1 个空格分开，输入保证句子末尾没有多余的空格。

## 输出格式： ##
	每个测试用例的输出占一行，输出倒序后的句子。

## 输入样例： ##
	Hello World Here I Come
## 输出样例： ##
	Come I Here World Hello

## 思路 ##
	使用gets函数读入一整行, 从左至右枚举每一个字符, 
	以空格为分隔符对单词进行划分并按顺序存放到二维字符数组中, 
	最后按单词输入顺序的逆序来输出所有单词.
## 注意事项 ##
- 最后一个单词之后输出空格会导致 "格式错误".
- 由于PAT是单点测试, 因此产生了下面这种更简洁的方法, 即使用EOF来判断单词是否已经输入完毕.
- 在小黑框手动输入时, 需要用<Ctrl+Z>组合键后按<Enter>键的方式来告诉系统已经到了EOF, 这样系统才会结束while.
- 此方法已注释, 需要删除注释才可以使用.

## 代码分析 ##
	#include <cstdio>
	#include <cstring> 
	
	int main(){
		//int num = 0; //单词的个数
		char ans[90][90]; 
		
		char str[90];
		gets(str);
		int len = strlen(str), row = 0, col = 0; //row 为行, col 为列.
		for(int i = 0; i < len; i++){
			if(str[i] != ' '){ //如果不是空格, 则存放至ans[row][col], 并令 col++
				ans[row][col++] = str[i]; 
			}else{ //如果是空格, 则说明一个单词结束, 行 row增加, 列 col恢复至 0.
				ans[row][col] = '\0'; //末尾是结束符\0 
				row++;
				col = 0; 
			}
		} 
		
		/*
		while(scanf("%s", ans[num]) != EOF){ //一直输入到文件末尾
			num++; //单词个数加一 
		} 
		
		
		for(int i = num - 1; i >= 0; i--){ //倒着输出单词 
			printf("%s", ans[i]);
			if(i > 0) printf(" "); 
		} 
		*/
		
		for(int i = row; i >= 0; i--){ //倒着输出单词 
			printf("%s", ans[i]);
			if(i > 0) printf(" "); 
		} 
		
		return 0;
	}
## 运行结果 ##
	Hello World Here I Come

	Come I Here World Hello
	--------------------------------
	Process exited with return value 0
	Press any key to continue . . .

----------
# B1040-有几个PAT #

## 描述 ##
	字符串 APPAPT 中包含了两个单词 PAT，
	其中第一个 PAT 是第 2 位(P)，第 4 位(A)，第 6 位(T)；
	第二个 PAT 是第 3 位(P)，第 4 位(A)，第 6 位(T)。
	
	现给定字符串，问一共可以形成多少个 PAT？

## 输入格式 ##
	输入只有一行，包含一个字符串，长度不超过10的	​5次方​，只包含 P、A、T 三种字母。

## 输出格式 ##
	在一行中输出给定字符串中包含多少个 PAT。由于结果可能比较大，只输出对 1000000007 取余数的结果。

## 输入样例 ##
	APPAPT

## 输出样例 ##
	2

## 实现代码 ##
	#include <cstdio>
	#include <cstring>
	#include <algorithm>
	
	const int MAXN = 100010;
	const int MOD = 1000000007;
	char str[MAXN]; 
	int leftNumP[MAXN] = {0}; //每一位左边（含）p的个数
	
	int main(){
		//读入字符串：gets(str)的替代方法。
	    int i=0;
	    fgets(str, MAXN, stdin);
	    while (str[i] != '\n')
	        i++;
	    str[i] = '\0';
	
	    int len = strlen(str); //长度
	    //printf("%d", len);
	
	    for (int i = 0; i < len; i++) //从左至右遍历字符串
	    {
	        if (i > 0) //如果不是0号位
	        {
	            leftNumP[i] = leftNumP[i-1];
	        }
	        if (str[i] == 'P') //当前位是P
	        {
	            leftNumP[i]++;
	        }     
	    }
	    //printf("%d\n", leftNumP[len-1]); //输出P的个数
	    
	
	    int ans = 0, rightNumT = 0; //ans为答案，rightNumT记录右边T的个数
	    for (int i = len-1; i >= 0; i--) //从右至左遍历字符串
	    {
	        if (str[i] == 'T')
	        {
	            rightNumT++;
	        }else if (str[i] == 'A')
	        {
	            ans = (ans + leftNumP[i] * rightNumT) % MOD; //累计乘积
	        }        
	    }
	    //printf("%d\n", rightNumT); //输出T的个数
	    
	    printf("%d\n", ans); //输出结果
	
	    return 0;
	}

----------
# B1019 数字黑洞 #

## 描述 ##
**给定任一个各位数字不完全相同的 4 位正整数，如果我们先把 4 个数字按非递增排序，再按非递减排序，然后用第 1 个数字减第 2 个数字，将得到一个新的数字。一直重复这样做，我们很快会停在有“数字黑洞”之称的 6174，这个神奇的数字也叫 Kaprekar 常数。**

例如，我们从6767开始，将得到

	7766 - 6677 = 1089
	9810 - 0189 = 9621
	9621 - 1269 = 8352
	8532 - 2358 = 6174
	7641 - 1467 = 6174
	... ...

现给定任意 4 位正整数，请编写程序演示到达黑洞的过程。

## 输入格式 ##
**输入给出一个 (0, 10​的4次方) 区间内的正整数N。**

## 输出格式 ##
**如果 N 的 4 位数字全相等，则在一行内输出 N - N = 0000；否则将计算的每一步在一行内输出，直到 6174 作为差出现，输出格式见样例。注意每个数字按 4 位数格式输出。**

## 实现代码 ##

	#include <cstdio>
	#include <algorithm>
	using namespace std;
	
	bool cmp(int a, int b){
	    return a > b; //递减排序：从大到小
	}
	
	//将num数组转换为数字
	int to_number(int num[]){
	    int sum = 0;
	    for (int i = 1; i <= num[0]; i++){
	        sum = sum * 10 + num[i];
	    }
	
	    return sum;
	}
	
	//将n的每一位存储到数组中，最多四位
	void to_array(int n, int num[]){
	    //eg: n=1024, 则num[5] = {4, 1, 0, 2, 4}, num[0]表示数组实际长度，此处为4
	    for (int i = num[0]; i >= 1; i--){
	        num[i] = n % 10; 
	        n /= 10;
	    }
	
	    /* //eg: n=1024, 则num[5] = {4, 4, 2, 1, 0}, num[0]表示数组实际长度，此处为4
	    for (int i = 1; i <= num[0]; i++){
	        num[i] = n % 10; 
	        n /= 10;
	    } */
	}
	
	int main(){
	    //MIN和MAX分别表示递增排序和递减排序后得到的最大值和最小值
	    int n, MIN, MAX;
	    scanf("%d", &n);
	
	    int num[5] = {4}, len = num[0]; //num[0]表示数组实际长度，此处为4。
	
	    while (1)
	    {
	        to_array(n, num);
	
	        //注：sort(首元素地址, 尾元素地址的下一地址, (可选)比较函数);
	        sort(num+1, num+len+1); //递增排序：从小到大排序
	        MIN = to_number(num);
	        //注：sort(首元素地址, 尾元素地址的下一地址, (可选)比较函数);
	        sort(num+1, num+len+1, cmp); //递减排序：从大到小排序
	        MAX = to_number(num);
	
	        n = MAX - MIN;
	        printf("%04d - %04d = %04d\n", MAX, MIN, n);
	
	        if(n == 0 || n == 6174) break; //退出条件
	    }
	    
	
	    return 0;
	}

----------
# codeup-1818 #

- 最大公约数
- 最小公倍数
- 分数的四则运算
- 找100以内的素数


		/*
		题目描述：输入两个正整数，求其最大公约数。
		输入：测试数据有多组，每组输入两个正整数。
		输出：对于每组输入,请输出其最大公约数。
		样例输入：49 14
		样例输出：7
		*/
		#include <cstdio>
		#include <algorithm>
		#include <cmath>
		
		//求最大公约数的辗转相除法递归写法
		int gcd(int a, int b) {
		    if(b == 0){
		        return a;
		    }else{
		        return gcd(b, a % b);
		    }
		}
		
		//在gcd的基础上求最小公倍数
		int lcm(int a, int b) {
		    int d = gcd(a, b);
		
		    //return (a * b) / d;
		    return a / d * b; //为防止溢出
		}
		
		/* 
		分数的表示：
		1. 使down为非负数。如果分数为负，那么令分子up为负即可。
		2. 如果该分数恰好为0，那么规定其分子为0，分母为1。
		3. 分子和分母没有除了1以外的公约数。
		*/
		struct Fraction { 
		    long long up; //分子
		    long long down; //分母
		}; //结构体要有分号
		
		/*
		分数的化简：
		1. 如果分母down为负数，那么令分子up和down都变为相反数。
		2. 如果分子up为0，那么令分母down为1。
		3. 约分：求出分子绝对值与分母绝对值的最大公约数d，然后令分子分母同时除以d。
		*/
		Fraction reduction(Fraction result) {
		    //如果分母down为负数，那么令分子up和down都变为相反数。
		    if (result.down < 0) 
		    {
		        result.up = (-result.up);
		        result.down = (-result.down);
		    }
		
		    //如果分子up为0，那么令分母down为1。
		    if (result.up == 0)
		    {
		        result.down = 1;
		    } else //如果分子不为0，进行约分
		    {
		        //求出分子绝对值与分母绝对值的最大公约数d，然后令分子分母同时除以d。
		        int d = gcd(abs(result.up), abs(result.down));
		        result.up /= d;
		        result.down /= d;
		    }    
		
		    return result;
		}
		
		//分数的加法
		Fraction add(Fraction f1, Fraction f2) {
		    Fraction result;
		    result.up = f1.up * f2.down + f2.up * f1.down;
		    result.down = f1.down * f2.down;
		
		    return reduction(result);
		}
		
		//分数的减法
		Fraction minu(Fraction f1, Fraction f2) {
		    Fraction result;
		    result.up = f1.up * f2.down - f2.up * f1.down;
		    result.down = f1.down * f2.down;
		
		    return reduction(result);
		}
		
		//分数的乘法
		Fraction multi(Fraction f1, Fraction f2) {
		    Fraction result;
		    result.up = f1.up * f2.up;
		    result.down = f1.down * f2.down;
		
		    return reduction(result);
		}
		
		//分数的除法
		Fraction devide(Fraction f1, Fraction f2) {
		    Fraction result;
		    result.up = f1.up * f2.down;
		    result.down = f1.down * f2.up;
		
		    return reduction(result);
		}
		
		//分数的输出
		void showResult(Fraction r) {
		    r = reduction(r);
		
		    if(r.down == 1) {
		        printf("%lld", r.up); //整数
		    }else if (abs(r.up) > r.down) { //假分数
		        printf("%d %d/%d", r.up/r.down, abs(r.up)%r.down, r.down);
		    }else //真分数
		    {
		        printf("%d/%d", r.up, r.down);
		    }
		    
		}
		
		//素数的判断：除了1和本身之外，不能被其他整数整除的一类数。
		bool isPrime(int n) {
		    //1既不是素数也不是合数
		    if (n <= 1) return false;
		    int sqr = sqrt(1.0 * n);
		    for (int i = 2; i <= sqr; i++)
		    {
		        if (n % i == 0) return false;
		    }
		    
		    return true;                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             
		}
		
		const int maxn = 101; //表长
		int prime[maxn], pNum = 0; //prime数组存放所有素数，pNum为素数个数
		bool p[maxn] = {0}; //如果i为素数，则p[i]为false；否则，p[i]为true。
		//求解100以内的所有素数，maxn <= 10的5次方。
		void Find_Prime() {
		    for (int i = 2; i < maxn; i++)
		    {
		        if (p[i] == false) //如果i为素数
		        {
		            prime[pNum++] = i; //是素数则把i存入prime数组
		            for (int j = i + i; j < maxn; j += i)
		            {
		                //筛去所有i的倍数
		                p[j] = true;
		            }
		            
		        }        
		    }    
		}
		
		
		
		int main() {
		    /* int m, n;
		    while (scanf("%d%d", &m, &n) != EOF) //最普遍地说，它是ASCII码中的替换字符（Control-Z，代码26）
		    {
		        printf("%d\n", gcd(m, n));
		        printf("%d\n", lcm(m, n));
		    } */
		
		    /* Fraction f1, f2;
		    f1.up = 8, f1.down = 9;
		    f2.up = 9, f2.down = 8;
		    Fraction f3 = multi(f1, f2); 
		    showResult(f3); */
		    
		    Find_Prime();
		    for (int i = 0; i < pNum; i++)
		    {
		        printf("%d ", prime[i]);
		    }
		    printf("\n");   
		
		    return 0;
		}

----------
# B1013-数素数 #

## 描述 ##
	令 P[​i] 表示第 i 个素数。
	现任给两个正整数 M≤N≤10的​4次方​，请输出 P[M​​] 到 P[​N​​]  的所有素数。

## 输入格式 ##
	输入在一行中给出 M 和 N，其间以空格分隔。

## 输出格式 ##
	输出从 P[​M​​] 到 P[​N]​​ 的所有素数，
	每 10 个数字占 1 行，其间以空格分隔，但行末不得有多余空格。
## 输入样例 ##
	5 27
## 输出样例 ##
	11 13 17 19 23 29 31 37 41 43
	47 53 59 61 67 71 73 79 83 89
	97 101 103
## 示例代码 ##
	#include <cstdio>
	
	const int maxn = 1000001; //表长
	int prime[maxn], num = 0; //prime数组存放所有素数，pNum为素数个数
	bool p[maxn] = {0}; //如果i为素数，则p[i]为false；否则，p[i]为true。
	//找n个素数
	void Find_Prime(int n) {
	    for (int i = 2; i < maxn; i++)
	    {
	        if (p[i] == false) //如果i为素数
	        {
	            prime[num++] = i; //是素数则把i存入prime数组
	            if (num >= n) break; //只需要n个素数
	            for (int j = i + i; j < maxn; j += i)
	            {
	                //筛去所有i的倍数
	                p[j] = true;
	            }
	            
	        }        
	    }    
	}
	
	//输出第M~N个素数（M <= N <= 10的4次方）
	int main() {
	    int m, n, count = 0;
	    scanf("%d%d", &m, &n);
	    Find_Prime(n);
	    for (int i = m; i <= n; i++)
	    {
	        //输出第m个素数至第n个素数
	        printf("%d", prime[i-1]); //下标从0开始
	        count++;
	        //每 10 个数字占 1 行，其间以空格分隔，但行末不得有多余空格。
	        if (count % 10 != 0 && i < n) 
	            printf(" ");
	        else
	            printf("\n");
	    }    
	
	    return 0;
	}

----------
# A1059 Prime Factors #

## 描述 ##
	Given any positive integer N, you are supposed to find all of its prime factors,
	and write them in the format N = p​1Lk1] * p2[k2] * ⋯ * pm * k[m].

## Input Specification: ##
	Each input file contains one test case 
	which gives a positive integer N in the range of long int.

## Output Specification: ##
	Factor N in the format N = p1^k1 * p2^k2 * … * pm^km, 
	where p​i's are prime factors of N in increasing order,
	and the exponent k​i is the number of p​i​​
	-- hence when there is only one p​i​​,
	k​i is 1 and must NOT be printed out.

## Sample Input: ##
	97532468

## Sample Output: ##
	97532468=2^2*11*17*101*1291

## 示例代码 ##

	#include <cstdio>
	#include <cmath>
	
	const int maxn = 100010;
	
	//判断n是否是素数
	bool is_Prime(int n) {
	    //1既不是素数也不是合数
	    if (n <= 1) return false;
	    int sqr = sqrt(1.0 * n);
	    for (int i = 2; i <= sqr; i++)
	    {
	        if (n % i == 0) return false;
	    }
	    
	    return true;                                                                                          
	}
	
	int prime[maxn], pNum = 0; 
	bool p[maxn] = {0}; //如果i为素数，则p[i]为false；否则，p[i]为true。
	//求素数表
	void Find_Prime() {
	    /* for (int i = 1; i < maxn; i++)
	    {
	        if (is_Prime(i) == true)
	        {
	            prime[pNum++] = i;
	        }
	        
	    }     */
		
		//改进代码
	    for (int i = 2; i < maxn; i++)
	    {
	        if (p[i] == false) //如果i为素数
	        {
	            prime[pNum++] = i; //是素数则把i存入prime数组
	            for (int j = i + i; j < maxn; j += i)
	            {
	                //筛去所有i的倍数
	                p[j] = true;
	            }
	            
	        }        
	    }    
	}
	
	//存放质因子
	struct fractor {
	    int x, cnt; //x为质因子，cnt为其个数
	}fac[10];
	
	//给出一个int范围的整数，按照从小到大的顺序输出其分解为质因数的乘法算式
	int main() {
	    Find_Prime();
	    int n, num = 0; //num为n的不同质因子的个数
	    scanf("%d", &n);
	    if(n == 1) printf("1 == 1"); //特判1的情况
	    else
	    {
	        printf("%d=", n);
	        int sqr = (int)sqrt(1.0 * n); //根号n
	        //枚举根号n以内的质因子
	        for (int i = 0; i < pNum && prime[i] <= sqr; i++)
	        {
	            //如果prime[i]是n的因子
	            if (n % prime[i] == 0)
	            {
	                fac[num].x = prime[i]; //记录该因子
	                fac[num].cnt = 0;
	
	                //计算出质因子prime[i]的个数
	                while (n % prime[i] == 0) 
	                {
	                    fac[num].cnt++;
	                    n /= prime[i];
	                }
	
	                num++; //不同质因子个数加1
	            }
	
	            if (n == 1) break; //及时退出循环，节省点时间
	        }
	
	        //如果在上面的操作结束后n仍然大于1，说明有且仅有一个大于sqrt(n)的质因子（有可能是n本身）
	        if (n != 1)
	        {
	            fac[num].x = n;
	            fac[num++].cnt = 1;
	        }
	
	        //按格式输出结果
	        for (int i = 0; i < num; i++)
	        {
	            if (i > 0) printf("*");
	            printf("%d", fac[i].x);
	            if (fac[i].cnt > 1)
	            {
	                printf("^%d", fac[i].cnt);
	            }            
	        }        
	    }    
	
	    return 0;
	}

----------
