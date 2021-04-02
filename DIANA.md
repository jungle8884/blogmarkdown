---
title: DIANA
categories:
  - 机器学习
  - 分类算法
tags:
  - Python
  - 层次聚类
date: 2020-03-06 16:48:46
author: Jungle

---

**DIANA算法代码如下:**

**源代码: https://github.com/jungle8884/DIANA**

1. **DIANA（Divisive Analysis）算法属于分裂的层次聚类**，首先将所有的对象初始化到一个簇中，然后根据一些原则（比如最邻近的最大欧式距离），将该簇分类。 直到到达用户指定的簇数目或者两个簇之间的距离超过了某个阈值。
	-  簇的直径：在一个簇中的任意两个数据点都有一个欧氏距离，这些距离中的最大值是簇的直径
	-  平均相异度（平均距离）

2.  算法描述：

		- 输入：包含n个对象的数据库，终止条件簇的数目k
		- 输出：k个簇，达到终止条件规定簇数目
				1. 将所有对象整个当成一个初始簇[将每个点当成一个初始簇]
				2. For (i=1;i!=k;i++) Do Begin
				3.      在所有簇中挑选出具有最大直径的簇C
				4.      找出C与其他点**平均相异度最大**的一个点P放入splinter group，
				5.      剩余的放入old party中。
				5.    Repeat
				6.      在old party里找出**到splinter group中点的最近距离** <= **到old party中点的最近距离的点**，并将该点加入splinter group
				7.    Until 没有新的old party的点被分配给splinter group；
				8. Splinter group 和old party为被选中的簇分裂成的两个簇，与其他簇一起组成新的簇集合
				9. END	
		- 算法性能: 缺点是已做的分裂操作不能撤销，类之间不能交换对象。如果在某步没有选择好分裂点，可能会导致低质量的聚类结果。大数据集不太适用。

3. 具体理解: 

	1. 数据: 
		- 序号 属性1 属性2
		-  1	 1    1
		-  2	 1	  2
		-  3     2    1
		-  4     2    2
		-  5	 3	  4
		-  6	 3	  5
		-  7	 4	  4
		-  8     4    5
	2.  实例分析:
		1. 对于所给的数据进行DIANA算法，（设n=8,用户输入的终止条件为两个簇）， 初始簇{1,2,3,4,5,6,7,8}。
		2. 第一步，找到具有最大直径的簇，对簇中的每个点计算平均相异度
			1. 序号1的平均距离（就是1距离其他各个点的距离长度之和除以7）
				- s1=(1+1+1.1414+3.6+4.24+4.47+5)/7=2.96; 
				- [这里取两位小数, 代码运行实际更精确.]
			2. 序列2的平均距离 s2=(1+1.414+1+2.828+3.6+3.6+4.24)/7=2.526;
			3. 序列3的平均距离 s3=(1+1.414+1+3.16+4.12+3.6+4.27)/7=2.68;
			4. 序列4的平均距离 s4=(1.414+1+1+2.24+3.16+2.828+3.6)/7=2.18
			5. 序列5的平均距离 s5=2.18;
			6. 序列6的平均距离 s6=2.68;
			7. 序列7的平均距离 s7=2.526;
			8. 序列8的平均距离 s8=2.96;
			9. 将8个点的平均距离中:
				- 挑选一个最大平均相异度的 点1 放入splinter group中
				- 剩余点放在old party中
				- splinter group={1}
				- old party={2,3,4,5,6,7,8}
		3. 第二步，在old party里找出**到splinter group的距离**小于**到old party的距离的点**，将该点放入splinter group中，该点就是2；
			- splinter group={1,2}, old party={3,4,5,6,7,8}
		4. 第三步，在old party里找出**到splinter group的距离**小于**到old party的距离的点**，将该点放在splinter group中，该点就是3；
			- spliner group={1,2,3}, old party={4,5,6,7,8}
		5. 第四步，在old party里找出**到splinter group的距离**小于**到old party的距离的点**，将该点放在splinter group中，该点就是4；
			- spliner group={1,2,3,4}, old party={5,6,7,8}
		6. 第五步, 在old party里找出**到splinter group的距离**小于**到old party的距离的点**。
			- 此时的分裂的簇数为2，已经达到了终止条件。
			- 如果还没达到终止条件，下一阶段还会在分裂好的簇中选一个直径最大的簇按刚才的分类方法分裂。
		

4. 代码实现: 
		
		import math
		import numpy as np
		import pylab as pl
	
	
	​	
	​	# 加载数据
	​	def loadData(filename):
	​	    dataSet = np.loadtxt(filename, delimiter='\t', encoding="UTF-8-sig")
	​	    return dataSet
	
	
	​	
	​	# 计算欧式距离
	​	def dist(X, Y):
	​	    # X 代表坐标(x1, x2)   Y 代表坐标(y1, y2)
	​	    return math.sqrt(math.pow(X[0] - Y[0], 2) + math.pow(X[1] - Y[1], 2))
	
	
	​	
	​	# 找到具有最大直径的簇
	​	def find_Maxdis(M):
	​	    avg_Dissimilarity = []  # 平均相异度数组
	​	    max_Dissimilarity = -1  # 最大平均相异度
	​	    numOrder = 0
	​	    # M 为 (n, n) 矩阵, 对每个点计算平均相异度
	​	    for i in range(len(M)):
	​	        dis = 0
	​	        for j in range(len(M[i])):
	​	            if i != j:
	​	                dis += M[i][j]
	​	        avg_Dissimilarity.append(dis / (len(M) - 1))
	​	        # print(str(avg_Dissimilarity[i])+ "    " + str(i))
	​	    # 在找到平均相异度的最大值
	​	    for m in range(len(avg_Dissimilarity)):
	​	        if max_Dissimilarity < avg_Dissimilarity[m]:
	​	            max_Dissimilarity = avg_Dissimilarity[m]
	​	            numOrder = m
	​	    # print(str(max_Dissimilarity) + "    " + str(k))
	​	
		    # 返回当前平均相异度最大的簇的序号和平均相异度
		    return numOrder, max_Dissimilarity
	
	
	​	
	​	def DIANA(dataSet, k):
	​	    '''第一步，找到具有最大直径的簇，对簇中的每个点计算平均相异度 ["找出最突出的点"],
	​		挑选一个最大平均相异度的点放入splinter group中, 剩余点放在old party中.'''
	​	    C = []  # 初始簇, 所有对象为一个簇
	​	    M = []  # 每个对象间的距离矩阵
	​	    for ci in dataSet:
	​	        # 先构造一维数组, 每一个对象为一个一维数组
	​	        c = []
	​	        c.append(ci)
	​	        # 再将一维数组加入到数组中, 便构成了二维数组.
	​	        C.append(ci)
	​	    for ci in dataSet:
	​	        Mi = []
	​	        for cj in dataSet:
	​	            Mi.append(dist(ci, cj))
	​	        M.append(Mi)
	​	    # 挑选一个最大平均相异度的点放入splinter group中, 剩余点放在old party中.
	​	    numOrder, max_Dissimilarity = find_Maxdis(M)
	​	    splinter_group = []
	​	    old_party = []
	​	    C_num = list(range(len(dataSet)))
	​	    splinter_group.append(numOrder)
	​	    C_num.remove(numOrder)
	​	    for num in C_num:
	​	        old_party.append(num)
	​	    '''第二步，在old party里找出   
	​	                        **到splinter group的距离**
	​	                            小于
	​	                        **到old party的距离的点**
	​	                    将该点放入splinter group中,
	​	            Repeat ["不停地找最突出的点加入splinter group"]
	​	            Until 没有新的old party的点被分配给splinter group；
	​	            Splinter group 和old party为被选中的簇分裂成的两个簇，与其他簇一起组成新的簇集合
	​	            END'''
	​	    i = 1
	​	    while i != k:
	​	        # 在old party里找出**到splinter group的距离**小于**到old party的距离的点**
	​	        dis_Old_Old = 0
	​	        dis_Old_Splinter = 0
	​	        for old in old_party:
	​	            print(splinter_group)
	​	            # print(old_party)
	​	
		            # 在old party里先找到old party的距离---这里指平均距离
		            for old_other in old_party:
		                # 自己到自己的距离为零, 所以不做其它处理
		                dis_Old_Old += M[old][old_other]
		            dis_Old_Old = dis_Old_Old / (len(old_party)-1)
		            print("dis_Old_Old: " + str(dis_Old_Old))
		
		            # 在old party里然后再找到splinter group的距离---这里指平均距离
		            for splinter in splinter_group:
		                dis_Old_Splinter += M[old][splinter]
		            dis_Old_Splinter = dis_Old_Splinter / (len(splinter_group))
		            print("dis_Old_Splinter: " + str(dis_Old_Splinter))
		
		            # 比较后, 将满足条件的点放在splinter group中
		            if dis_Old_Splinter < dis_Old_Old:
		                splinter_group.append(old)
		                print("每次加入到splinter_group的点: "+str(old))
		            else:
		                print("不满足条件的点: " + str(old))
		            print("\n")
		        i = i + 1
	
	
	​	
	​	if __name__ == "__main__":
	​	    dataSet = loadData('test.txt')
	​	    # print(dataSet)
	​	    k = 2  # k代表预设的最终聚类簇数
	​	    DIANA(dataSet, k)


​	