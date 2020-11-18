---
title: 算法笔记-排序
categories:
  - 算法笔记
tags:
  - C++
  - 算法
  - 排序
date: 2020-10-15 18:17:02
author: Jungle

---
# 冒泡排序 #

## 代码 ##
		#include <cstdio> 

		/*数组作为参数时, 第一维不需要填写长度;
		实际调用时也只需要填写数组名;
		数组作为参数时, 在函数中对数组元素的修改就等同于是对原数组元素的修改.*/

		void BubbleSort(int n, int arry[]){
			for(int i = 0; i < n-1; i++){
				for(int j = 0; j < n-i; j++){
					if(arry[j] > arry[j+1]){
						int temp = arry[j];
						arry[j] = arry[j+1];
						arry[j+1] = temp;
					}
				}
			}
		}
		
		int main(){
			int a[5] = {3, 1, 4, 5, 2};
			BubbleSort(5, a);
			for(int i = 0; i < 5; i++){
				printf("%d\t", a[i]);
			}
			
			return 0;
		}

## 运行结果 ##
		1       2       3       4       5
		--------------------------------
		Process exited with return value 0
		Press any key to continue . . .

----------

# 选择排序 #

## 代码 ##
		#include <cstdio>

		void SelectSort(int n, int A[]){
			for(int i = 0; i < n; i++){ //进行 n 趟操作 
				int k = i;
				for(int j = i; j < n; j++){ //每趟操作找出待排序部分[i, n]中最小的元素. 
					if(A[j] < A[k]){
						k = j;
					}
				}
				//k 是本趟操作中最小元素的 下标, A[k]与 第一个元素A[i]交换. 
				int temp = A[i];
				A[i] = A[k];
				A[k] = temp; 
			}
		}
		
		int main(){
			int a[5] = {3, 1, 4, 5, 2};
			SelectSort(5, a);
			for(int i = 0; i < 5; i++){
				printf("%d\t", a[i]);
			}
			
			return 0;
		} 

## 运行结果 ##
		1       2       3       4       5
		--------------------------------
		Process exited with return value 0
		Press any key to continue . . .	

----------
# 插入排序 #

## 代码 ##
		#include <cstdio>
		// #include <algorithm>
		// using namespace std;
		
		void InsertSort(int n, int A[]){
			for(int i = 1; i < n; i++){ //进行 n-1 趟
				int temp = A[i], j = i; //temp 临时存放 A[i], j 从 i 开始枚举
				while(j > 0 && temp < A[j-1]){ //只要 temp 小于前一个元素 A[j-1] 
					A[j] = A[j-1]; //把 A[j-1} 后移一位至 A[j} 
					j--;
				}
				A[j] = temp; //插入位置为j 
			}
		}
		
		int main(){
			int a[6] = {5, 2, 4, 6, 3, 1};
			InsertSort(6, a);
			//sort(a, a+6); //可以直接使用 c++ 自带的函数进行排序, 记得加上 #include <algorithm> 和  using namespace std;
			for(int i = 0; i < 6; i++){
				printf("%d\t", a[i]);
			}
			
			return 0;
		}

## 运行结果 ##
		1       2       3       4       5       6
		--------------------------------
		Process exited with return value 0
		Press any key to continue . . .

----------
# A1025 #

## 题目描述 ##
- Programming Ability Test (PAT) is organized by the College of Computer Science and Technology of Zhejiang University.
-  Each test is supposed to run simultaneously in several places, and the ranklists will be merged immediately after the test. 
-  Now it is your job to write a program to correctly merge all the ranklists and generate the final rank.

## 输入格式 ##
- Each input file contains one test case.
-  For each case, the first line contains a positive number N (≤100), the number of test locations. 
-  Then N ranklists follow, each starts with a line containing
	-  a positive integer K (≤300), the number of testees, 
	-  and then K lines containing 
		-  the registration number (a 13-digit number) 
		-  and the total score of each testee. 
-  All the numbers in a line are separated by a space.

## 输出格式 ##
- For each test case, first print in one line the total number of testees. 
- Then print the final ranklist in the following format:

		registration_number final_rank location_number local_rank

- The locations are numbered from 1 to N. 
- The output must be sorted in nondecreasing order of the final ranks. 
- The testees with the same score must have the same rank, and the output must be sorted in nondecreasing order of their registration numbers.

## 输入样例 ##
	2
	5
	1234567890001 95
	1234567890005 100
	1234567890003 95
	1234567890002 77
	1234567890004 85
	4
	1234567890013 65
	1234567890011 25
	1234567890014 100
	1234567890012 85

## 输出样例 ##
	9
	1234567890005 1 1 1
	1234567890014 1 2 1
	1234567890001 3 1 2
	1234567890003 3 1 2
	1234567890004 5 1 4
	1234567890012 5 2 2
	1234567890002 7 1 5
	1234567890013 8 2 3
	1234567890011 9 2 4

## 代码分析 ##
1. 可以用结构体类型Student存放题目要求的信息: 
	- 准考证号: registration_number
	- 分数: score
	- 总排名: final_rank
	- 考场号: location_number
	- 考场内排名: local_rank
2. 可以使用 C++ 自带的 sort() 函数
	- 需要引入 #include <algorithm>
	- 需要写一个排序函数 cmp 传入sort 使用.
3. 算法步骤:
	- 步骤一:
		- 按考场读入各考生的信息, 并对当前读入考场的所有考生进行排序. 
		- 之后将该考场的所有考生的排名写入他们的结构体中.
	- 步骤二: 对所有考生进行排序.
	- 步骤三: 
		- 按顺序一边计算总排名,
		- 一边输出所有考生的信息.

## 代码实现 ##

		#include <cstdio>
		#include <cstring>
		#include <algorithm>
		using namespace std; 
		
		struct Student {
			char registration_number[15]; //准考证号
			int score; //分数
			int location_number; //考场号 
			int local_rank; //考场内排名 
			int final_rank; //总排名 
		}stu[30010]; 
		
		bool cmp(Student a, Student b){
			if (a.score != b.score)  
				return a.score > b.score; //先按分数从高到低进行排序
			else 
				return strcmp(a.registration_number, b.registration_number) < 0; //分数相同按准考证号从小到大排序 
		}
		
		
		
		int main(){
			
			int n, k, num = 0; //num为总考生人数, k代表该考场的考生人数 
			scanf("%d", &n); //n为考场数 
			
			//步骤一 
			for(int i = 1; i <= n; i++){
				scanf("%d", &k); //该考场人数
				for(int j = 0; j < k; j++){
					scanf("%s %d", stu[num].registration_number, &stu[num].score);
					stu[num].location_number = i; //该考生的考场号为i
					num++; 
				} 
				sort(stu+num-k, stu+num, cmp); //将该考场的考生排序
				stu[num-k].local_rank = 1; //该考场第一名的local_rank记为1 
				printf("%s ", stu[0].registration_number);
				printf("%d %d\n", stu[0].location_number, stu[0].local_rank);
				for(int j = num-k+1; j < num; j++){ //对该考场剩余的考生 
					if(stu[j].score == stu[j-1].score){ //如果与前一位考生同分
						//local_rank也相同
						stu[j].local_rank = stu[j-1].local_rank; 
					} else{ //如果与前一位考生不同分 
						//local_rank为该考生前的人数 
						stu[j].local_rank = j+1 - (num-k); //j+1是下标从0开始;  - (num-k)是为了抵消前面for循环赋值. 
					}
					printf("%s ", stu[j].registration_number);
					printf("%d %d\n", stu[j].location_number, stu[j].local_rank);
				} 
			} 
			printf("%d\n", num); //输出总考生人数
			//步骤二 
			sort(stu, stu+num, cmp); //将所有考生排名 
			//步骤三 
			int r = 1; //当前考生的排名
			for(int i = 0; i < num; i++){
				stu[i].final_rank = r; //当前考生的总排名 
				if(i > 0 && stu[i].score != stu[i-1].score){ //从stu[1], 第二个考生开始判断. 
					r = i+1; //当前考生与上一个考生分数不同时, 让r更新为人数+1. 
					stu[i].final_rank = r; //更新总排名 
				} 
				printf("%s ", stu[i].registration_number);
				printf("%d %d %d\n", stu[i].final_rank, stu[i].location_number, stu[i].local_rank); 
			} 
			
			return 0;
		} 

运行结果: 
	
		2
		5
		1234567890001 95
		1234567890005 100
		1234567890003 95
		1234567890002 77
		1234567890004 85
		4
		1234567890013 65
		1234567890011 25
		1234567890014 100
		1234567890012 85
		9
		1234567890005 1 1 1
		1234567890014 1 2 1
		1234567890001 3 1 2
		1234567890003 3 1 2
		1234567890004 5 1 4
		1234567890012 5 2 2
		1234567890002 7 1 5
		1234567890013 8 2 3
		1234567890011 9 2 4
		
		--------------------------------
		Process exited with return value 0
		Press any key to continue . . .

----------
