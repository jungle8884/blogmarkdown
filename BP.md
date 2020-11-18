---
title: 误差反向传播算法(BackPropagation算法)
categories:
  - 机器学习
tags:
  - 人工神经网络
date: 2020-03-19 14:49:40
author: Jungle

---

**下载代码: https://github.com/jungle8884/BP**

1. BP神经网络:
	BP神经网络是这样一种神经网络模型，它是由一个输入层、一个输出层和一个或多个隐层构成，它的激活函数采用sigmoid函数，采用BP算法训练的多层前馈神经网络

2. BP算法基本思想:
	- BP算法全称叫作误差反向传播(error Back Propagation，或者也叫作误差逆传播)算法。
	- 在前馈网络中，输入信号经输入层输入，通过隐层计算由输出层输出，输出值与标记值比较，若有误差，将误差反向由输出层向输入层传播，在这个过程中，利用梯度下降算法对神经元权值进行调整。
				
----------
	-  BP算法步骤:
		1. 确定输入模式向量和理想输出向量
		2. 确定隐含层的层数和每层的单元（神经元、节点）数
		3. 给各单元的连接权值和阈值赋予（-1，+1）间的随机数
		4. 选择某个训练样本向量作为输入，进行学习，计算输出
		5. 根据实际输出与理想输出的偏差，修正各单元的连接权值和阈值
		6. 选择下一个训练样本向量作为输入，再进行学习，计算输出
		7. 重复5、6步骤，通过多个样本的反复训练，不断修正各单元的连接权值和阈值，直至网络全局误差函数小于预先设定的极小值，学习结束
		8. 应用测试样本检查模式识别的准确性
		

----------

3. 以 **BP神经网络加法运算** 为例, 理解BP算法.
	1. 详细理论过程见下: 
		- https://www.cnblogs.com/jzhlin/archive/2012/07/28/bp.html
		- https://www.cnblogs.com/jzhlin/archive/2012/07/30/bp_c.html
		- https://www.cnblogs.com/jzhlin/archive/2012/08/01/bp_c2.html

	2. 理论推导: https://blog.csdn.net/jungle8884/article/details/104981584
	3. 示例代码:
			
				// neuron.cpp : 定义控制台应用程序的入口点。
				//
				
				//#include "stdafx.h"
				#include <stdio.h>
				#include <time.h>
				#include <math.h>
				#include <stdlib.h>
				
				#define Data  1000  	// 训练样本的数量
				#define In 2			// 对于每个样本有多少个输入变量 
				#define Out 1			// 对于每个样本有多少个输出变量
				#define Neuron 5		// 神经元的数量 
				#define TrainC 20000 	// 表示训练的次数 
				#define A  0.2			
				#define B  0.4
				#define a  0.2
				#define b  0.3
				
				// d_in[Data][In] 存储 Data 个样本，每个样本的 In 个输入
				// d_out[Data][Out] 存储 Data 个样本，每个样本的 Out 个输出
				double d_in[Data][In],d_out[Data][Out]; 
					
				// w[Neuron][In]  表示某个输入对某个神经元的权重 
				// v[Out][Neuron] 来表示某个神经元对某个输出的权重 
				// 数组 o[Neuron] 记录的是神经元通过激活函数对外的输出 
				// 与之对应的保存它们两个修正量的数组 dw[Neuron][In] 和 dv[Out][Neuron] 
				double w[Neuron][In],v[Out][Neuron],o[Neuron];
				double dv[Out][Neuron],dw[Neuron][In];
				
				// Data个数据中 加数、被加数、和 的最大,最小值 
				double Maxin[In],Minin[In],Maxout[Out],Minout[Out];
				
				// OutputData[Out]  存储BP神经网络的输出 
				double OutputData[Out];
				
				// e用来监控误差 
				double e;
				
				//生成实验数据并存入相应文件中
				void writeTest(){      
					FILE *fp1,*fp2;
					double r1,r2;
					int i;
				
					if((fp1=fopen("E:\\neuron\\in.txt","w"))==NULL){
						printf("can not open the in file\n");
						exit(0);
					}
					if((fp2=fopen("E:\\neuron\\out.txt","w"))==NULL){
						printf("can not open the out file\n");
						exit(0);
					}
				
					for(i=0;i<Data;i++){
						// 生成0~10的随机小数 
						r1=rand()%1000/100.0;
						r2=rand()%1000/100.0; 
						// 写入文件 
						fprintf(fp1,"%lf  %lf\n",r1,r2);
						fprintf(fp2,"%lf \n",r1+r2);
					}
					fclose(fp1);
					fclose(fp2);
				}
				
				// 读入训练数据 
				void readData(){
				
					FILE *fp1,*fp2;
					int i,j;
					if((fp1=fopen("E:\\neuron\\in.txt","r"))==NULL){
						printf("can not open the in file\n");
						exit(0);
					}
					// 读入数据到 d_in[Data][In] 
					for(i=0;i<Data;i++)
						for(j=0; j<In; j++)
							fscanf(fp1,"%lf",&d_in[i][j]);
					fclose(fp1);
					
					if((fp2=fopen("E:\\neuron\\out.txt","r"))==NULL){
						printf("can not open the out file\n");
						exit(0);
					}
					// 读入数据到 d_in[Data][Out] 
					for(i=0;i<Data;i++)
						for(j=0; j<Out; j++)
							fscanf(fp1,"%lf",&d_out[i][j]);
					fclose(fp2);
				}
				
				/*
				一方面是对读取的训练样本数据进行归一化处理，
				归一化处理就是指的就是将数据转换成0~1之间; 
				在BP神经网络理论里面，并没有对这个进行要求，
				不过实际实践过程中，归一化处理是不可或缺的。
				因为理论模型没考虑到，BP神经网络收敛的速率问题，
				一般来说神经元的输出对于0~1之间的数据非常敏感，归一化能够显著提高训练效率。
				可以用以下公式来对其进行归一化，
				其中 加个常数A 是为了防止出现 0 的情况（0不能为分母）。
				另一方面，就是对神经元的权重进行初始化了，
				数据归一到了（0~1）之间，
				那么权重初始化为（-1~1）之间的数据，
				另外对修正量赋值为0 
				*/ 
				void initBPNework(){
				
					int i,j;
				
					for(i=0; i<In; i++){   //求Data个数据中 加数和被加数的最大、最小值。
						Minin[i]=Maxin[i]=d_in[0][i];
						for(j=0; j<Data; j++)
						{
							Maxin[i]=Maxin[i]>d_in[j][i]?Maxin[i]:d_in[j][i];
							Minin[i]=Minin[i]<d_in[j][i]?Minin[i]:d_in[j][i];
						}
					}
				
					for(i=0; i<Out; i++){     //求Data个数据中和的最大、最小值。
						Minout[i]=Maxout[i]=d_out[0][i];
						for(j=0; j<Data; j++)
						{
							Maxout[i]=Maxout[i]>d_out[j][i]?Maxout[i]:d_out[j][i];
							Minout[i]=Minout[i]<d_out[j][i]?Minout[i]:d_out[j][i];
						}
					}
					
					//输入数据归一化
					for (i = 0; i < In; i++)
						for(j = 0; j < Data; j++)
							d_in[j][i]=(d_in[j][i]-Minin[i]+1)/(Maxin[i]-Minin[i]+1);
							
					//输出数据归一化
					for (i = 0; i < Out; i++)    
						for(j = 0; j < Data; j++)
							d_out[j][i]=(d_out[j][i]-Minout[i]+1)/(Maxout[i]-Minout[i]+1);
					
					//初始化神经元
					for (i = 0; i < Neuron; ++i)	
						for (j = 0; j < In; ++j){	
							// rand()不需要参数，它会返回一个从0到最大随机数的任意整数 
							// rand()/RAND_MAX 为 (0, 1) 
							w[i][j]=rand()*2.0/RAND_MAX-1; // 权值初始化 
							dw[i][j]=0;
						}
				
						for (i = 0; i < Neuron; ++i)	
							for (j = 0; j < Out; ++j){
								v[j][i]=rand()*2.0/RAND_MAX-1; // 权值初始化 
								dv[j][i]=0;
							}
				}
				
				void computO(int var){   //第var组数据在隐藏层和输出层的输出结果o[]和outputdata[]。
				
					int i,j;
					double sum,y;
					// 神经元输出 
					for (i = 0; i < Neuron; ++i){
						sum=0;
						for (j = 0; j < In; ++j)
							sum+=w[i][j]*d_in[var][j];
						//Sigmoid 函数---激活函数 
						o[i]=1/(1+exp(-1*sum));
					}
				
					/*  隐藏层到输出层输出 */
					for (i = 0; i < Out; ++i){
						sum=0;
						for (j = 0; j < Neuron; ++j)
							sum+=v[i][j]*o[j];
						OutputData[i]=sum;
					}	
				}
				
				//从后向前更新权值；
				void backUpdate(int var)
				{
					int i,j;
					double t;
					for (i = 0; i < Neuron; ++i)
					{
						t=0;
						for (j = 0; j < Out; ++j){
							t+=(OutputData[j]-d_out[var][j])*v[j][i];
							
							/*
							 在具体实现对误差修改中，我们再加上学习率，
							 并且对先前学习到的修正误差量进行继承，
							 直白的说就是都乘上一个0到1之间的数
							*/ 
							dv[j][i]=A*dv[j][i]+B*(OutputData[j]-d_out[var][j])*o[i];
							v[j][i]-=dv[j][i];
						}
				
						for (j = 0; j < In; ++j){
							dw[i][j]=a*dw[i][j]+b*t*o[i]*(1-o[i])*d_in[var][j];
							w[i][j]-=dw[i][j];
						}
					}
				}
				
				double result(double var1,double var2)
				{
					int i,j;
					double sum,y;
				
					var1=(var1-Minin[0]+1)/(Maxin[0]-Minin[0]+1);
					var2=(var2-Minin[1]+1)/(Maxin[1]-Minin[1]+1);
				
					for (i = 0; i < Neuron; ++i){
						sum=0;
						sum=w[i][0]*var1+w[i][1]*var2;
						o[i]=1/(1+exp(-1*sum));
					}
					sum=0;
					for (j = 0; j < Neuron; ++j)
						sum+=v[0][j]*o[j];
				
					return sum*(Maxout[0]-Minout[0]+1)+Minout[0]-1;  //返归一化
				}
				
				void writeNeuron()
				{
					FILE *fp1;
					int i,j;
					if((fp1=fopen("E:\\neuron\\neuron.txt","w"))==NULL)
					{
						printf("can not open the neuron file\n");
						exit(0);
					}
					for (i = 0; i < Neuron; ++i)	
						for (j = 0; j < In; ++j){
							fprintf(fp1,"%lf ",w[i][j]);
						}
					fprintf(fp1,"\n\n\n\n");
				
					for (i = 0; i < Neuron; ++i)	
						for (j = 0; j < Out; ++j){
							fprintf(fp1,"%lf ",v[j][i]);
						}
				
					fclose(fp1);
				}
				
				
				/*两个输入a、b（10以内的数），一个输出 c，c=a+b。
				换句话说就是教BP神经网络加法运算 */
				void  trainNetwork(){
				
					int i,c=0,j;
					do{
						e=0;
						for (i = 0; i < Data; ++i){
							computO(i);//计算隐藏层和输出层所有神经元的输出。
							
							for (j = 0; j < Out; ++j)
								e+=fabs((OutputData[j]-d_out[i][j])/d_out[i][j]);//fabs（）对float，double求绝对值 
								
							backUpdate(i);//反向修改权值
						}
						printf("%d  %lf\n",c,e/Data);
						c++;
					}while(c<TrainC && e/Data>0.01);//一直训练到规定次数或平均误差小于0.01时结束。
				}
				
				//int _tmain(int argc, _TCHAR* argv[])
				int main(int argc, char* argv[])
				{
					writeTest();//随机生成Data个数据，每个数据包括两个个位数以及这两个数之和。
					readData();//准备输入，输出训练数据。
					initBPNework();//输入、输出数据归一化，以及网络权值初始化。
					trainNetwork();//训练网络。
					printf("%lf \n",result(6,8) );
					printf("%lf \n",result(2.1,7) );
					printf("%lf \n",result(4.3,8) );
					writeNeuron();//保存权值。
					getchar();
					return 0;
				
				}

----------
4. 完成课堂作业
	- 问题的提出:
		齿轮传动在运行过程中经常会出现各种类型的故障，如齿面擦伤、胶合、点蚀、裂纹、局部断齿等，均会引起振动烈度增加。如何根据所测振动信号自动识别故障的类型，是目前齿轮故障诊断研究的一个重点内容，属于模式识别问题。
		
	- 思路:
		- 神经网络的输入向量为各频带的相对能量，个数为8个。
		- 隐层节点数量选择为6个。
		- 网络的目标输出向量为2种状态：齿轮不存在点蚀故障（1 0），齿轮存在点蚀故障（0 1）。
      	- 对两种齿轮情况分别采集测试信号50组，组成网络训练样本100组，
      	- 另外各取 5 组作为网络测试样本。
      	- 各连接权值和节点阈值的初始值可赋予（-1，+1）间的随机数。
      	- 设置网络最大训练步数1000，训练目标误差为0.001。 

	

----------
	[下载请到: https://github.com/jungle8884/BP]
	- 完成代码:
		
					#include <stdio.h>
					#include <time.h>
					#include <math.h>
					#include <stdlib.h>
					
					#define Data  100  	// 训练样本的数量
					#define TestData 10		// 测试样本的数量 
					#define In 8			// 对于每个样本有多少个输入变量 
					#define Out 2			// 对于每个样本有多少个输出变量
					#define Neuron 6		// 神经元的数量 
					#define TrainC 1000 	// 表示训练的次数 
					#define A  0.2			
					#define B  0.4
					#define a  0.2
					#define b  0.3
					
					// d_in[Data][In] 存储 Data 个样本，每个样本的 In 个输入
					// d_out[Data][Out] 存储 Data 个样本，每个样本的 Out 个输出
					double d_in[Data][In],d_out[Data][Out]; 
					
					// d_test[TestData][In] 存储 TestData 个样本，每个样本的 In 个输入
					double d_test[TestData][In]; 
						
					// w[Neuron][In]  表示某个输入对某个神经元的权重 
					// v[Out][Neuron] 来表示某个神经元对某个输出的权重 
					// 数组 o[Neuron] 记录的是神经元通过激活函数对外的输出 
					// 与之对应的保存它们两个修正量的数组 dw[Neuron][In] 和 dv[Out][Neuron] 
					double w[Neuron][In],v[Out][Neuron],o[Neuron];
					double dv[Out][Neuron],dw[Neuron][In];
					
					// Data个数据中 加数、被加数、和 的最大,最小值 
					double Maxin[In],Minin[In],Maxout[Out],Minout[Out];
					
					// OutputData[Out]  存储BP神经网络的输出 
					double OutputData[Out];
					
					// e用来监控误差 
					double e;
					
					// 读入训练数据 
					void readData(){
					
						FILE *fp1,*fp2;
						int i,j;
						if((fp1=fopen("E:\\whealgear\\in.txt","r"))==NULL){
							printf("can not open the in file\n");
							exit(0);
						}
						// 读入数据到 d_in[Data][In] 
						for(i=0;i<Data;i++)
							for(j=0; j<In; j++)
								fscanf(fp1,"%lf",&d_in[i][j]);
						fclose(fp1);
						
						if((fp2=fopen("E:\\whealgear\\out.txt","r"))==NULL){
							printf("can not open the out file\n");
							exit(0);
						}
						// 读入数据到 d_in[Data][Out] 
						for(i=0;i<Data;i++)
							for(j=0; j<Out; j++)
								fscanf(fp1,"%lf",&d_out[i][j]);
						fclose(fp2);
					}
					
					
					void initBPNework(){
					
						int i,j;
					
						for(i=0; i<In; i++){   //求Data个数据中 加数和被加数的最大、最小值。
							Minin[i]=Maxin[i]=d_in[0][i];
							for(j=0; j<Data; j++)
							{
								Maxin[i]=Maxin[i]>d_in[j][i]?Maxin[i]:d_in[j][i];
								Minin[i]=Minin[i]<d_in[j][i]?Minin[i]:d_in[j][i];
							}
						}
					
						for(i=0; i<Out; i++){     //求Data个数据中和的最大、最小值。
							Minout[i]=Maxout[i]=d_out[0][i];
							for(j=0; j<Data; j++)
							{
								Maxout[i]=Maxout[i]>d_out[j][i]?Maxout[i]:d_out[j][i];
								Minout[i]=Minout[i]<d_out[j][i]?Minout[i]:d_out[j][i];
							}
						}
						
						//输入数据归一化
						for (i = 0; i < In; i++)
							for(j = 0; j < Data; j++)
								d_in[j][i]=(d_in[j][i]-Minin[i]+1)/(Maxin[i]-Minin[i]+1);
								
						//输出数据归一化
						for (i = 0; i < Out; i++)    
							for(j = 0; j < Data; j++)
								d_out[j][i]=(d_out[j][i]-Minout[i]+1)/(Maxout[i]-Minout[i]+1);
						
						//初始化神经元
						for (i = 0; i < Neuron; ++i)	
							for (j = 0; j < In; ++j){	
								w[i][j]=rand()*2.0/RAND_MAX-1; // 权值初始化 
								dw[i][j]=0;
							}
					
							for (i = 0; i < Neuron; ++i)	
								for (j = 0; j < Out; ++j){
									v[j][i]=rand()*2.0/RAND_MAX-1; // 权值初始化 
									dv[j][i]=0;
								}
					}
					
					void computO(int var){   //第var组数据在隐藏层和输出层的输出结果o[]和outputdata[]。     
					
						int i,j;
						double sum,y;
						// 神经元输出 
						for (i = 0; i < Neuron; ++i){
							sum=0;
							for (j = 0; j < In; ++j)
								sum+=w[i][j]*d_in[var][j];
							//Sigmoid 函数---激活函数 
							o[i]=1/(1+exp(-1*sum));
						}
					
						/*  隐藏层到输出层输出 */
						for (i = 0; i < Out; ++i){
							sum=0;
							for (j = 0; j < Neuron; ++j)
								sum+=v[i][j]*o[j];
							OutputData[i]=sum;
						}	
					}
					
					//从后向前更新权值；
					void backUpdate(int var)
					{
						int i,j;
						double t;
						for (i = 0; i < Neuron; ++i)
						{
							t=0;
							for (j = 0; j < Out; ++j){
								t+=(OutputData[j]-d_out[var][j])*v[j][i];
								
								/*
								 在具体实现对误差修改中，我们再加上学习率，
								 并且对先前学习到的修正误差量进行继承，
								 直白的说就是都乘上一个0到1之间的数
								*/ 
								dv[j][i]=A*dv[j][i]+B*(OutputData[j]-d_out[var][j])*o[i];
								v[j][i]-=dv[j][i];
							}
					
							for (j = 0; j < In; ++j){
								dw[i][j]=a*dw[i][j]+b*t*o[i]*(1-o[i])*d_in[var][j];
								w[i][j]-=dw[i][j];
							}
						}
					}
					
					void result(double var[In])
					{
						int i,j,k;
						double sum;
						double y[Out];
						
						for(i = 0; i < In; ++i){
							var[i]=(var[i]-Minin[i]+1)/(Maxin[i]-Minin[i]+1);
						}
					
						for (i = 0; i < Neuron; ++i){
							sum=0;
							for(k = 0; k < In; ++k){
								sum += w[i][k] * var[k];
							}
							o[i]=1/(1+exp(-1*sum));
						}
						
						for (k = 0; k < Out; ++k){
							sum=0;
							for (j = 0; j < Neuron; ++j){
								sum += v[k][j] * o[j];
							}
							y[k] = sum;
						}
						
						//返归一化
						for (k = 0; k < Out; ++k){
							y[k] = y[k] * (Maxout[0]-Minout[0]+1)+Minout[0]-1;
						}
						printf("%lf %lf\n", y[0], y[1]);
						
						return;  
					}
					
					void writeNeuron()
					{
						FILE *fp1;
						int i,j;
						if((fp1=fopen("E:\\whealgear\\whealgear.txt","w"))==NULL)
						{
							printf("can not open the neuron file\n");
							exit(0);
						}
						for (i = 0; i < Neuron; ++i)	
							for (j = 0; j < In; ++j){
								fprintf(fp1,"%lf ",w[i][j]);
							}
						fprintf(fp1,"\n\n\n\n");
					
						for (i = 0; i < Neuron; ++i)	
							for (j = 0; j < Out; ++j){
								fprintf(fp1,"%lf ",v[j][i]);
							}
					
						fclose(fp1);
					}
					
					
					void  trainNetwork(){
					
						int i,c=0,j;
						do{
							e=0;
							for (i = 0; i < Data; ++i){
								computO(i);//计算隐藏层和输出层所有神经元的输出。
								
								for (j = 0; j < Out; ++j)
									e+=fabs((OutputData[j]-d_out[i][j])/d_out[i][j]);//fabs（）对float，double求绝对值 
									
								backUpdate(i);//反向修改权值
							}
							//printf("%d  %lf\n",c, e/Data);
							c++;
						}while(c<TrainC && e/Data>0.001);//一直训练到规定次数或平均误差小于0.001时结束。
					}
					
					void testNetwork()
					{
						FILE *fp1,*fp2;
						int i,j;
						double test[In]; 
						if((fp1=fopen("E:\\whealgear\\test.txt","r"))==NULL){
							printf("can not open the in file\n");
							exit(0);
						}
						// 读入数据到 d_test[Data][In] 
						for(i=0;i<TestData;i++)
							for(j=0; j<In; j++)
								fscanf(fp1,"%lf",&d_test[i][j]);
						fclose(fp1);
						
						for(i=0;i<TestData;i++){
							for(j=0; j<In; j++){
								test[j] = d_test[i][j];
							}
							result(test);	
						}
					} 
					
					int main(int argc, char* argv[])
					{
						readData();//准备输入，输出训练数据。
						initBPNework();//输入、输出数据归一化，以及网络权值初始化。
						trainNetwork();//训练网络。
						testNetwork();//测试网络 
						writeNeuron();//保存权值。
						
						getchar();
						return 0;
					
					}

							  

		- 输出:
			
				0.817465 0.189203
				0.882086 0.135193
				0.649437 0.352534
				0.854933 0.155281
				0.805200 0.200115
				0.003144 1.002565
				0.111812 0.925652
				0.001671 1.001391
				-0.004160 1.007648
				-0.000645 0.997763

----------
