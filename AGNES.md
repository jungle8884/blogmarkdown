---
title: AGNES
categories:
  - 机器学习
tags:
  - 层次聚类
date: 2020-03-03 14:01:31
author: Jungle

---
**AGNES算法代码如下:**

**源代码: https://github.com/jungle8884/AGNES**

1. AGNES（自底向上凝聚算法）算法的具体步骤如下所示：

		输入：包含n个对象的数据库。
		输出：满足终止条件的若干个簇。
		(1) 将每个对象当成一个初始簇；
		(2) REPEAT
		(3)     计算任意两个簇的距离，并找到最近的两个簇；
		(4)     合并两个簇，生成新的簇的集合；
		(5) UNTIL 终止条件得到满足。
		
		- 层次聚类方法的终止条件：
			- 设定一个最小距离阈值d，如果最相近的两个簇的距离已经超过d，则它们不需再合并，聚类终止.
			- 限定簇的个数k，当得到的簇的个数已经达到k，则聚类终止.

		

2. 代码实现: 

			import math
			import numpy as np
			import pylab as pl
		

				# 加载数据
				def loadData(filename):
				    dataSet = np.loadtxt(filename, delimiter='\t', encoding="UTF-8-sig")
				    return dataSet
			
			
				# 计算欧式距离
				def dist(X, Y):
				    # X 代表坐标(x1, x2)   Y 代表坐标(y1, y2)
				    return math.sqrt(math.pow(X[0] - Y[0], 2) + math.pow(X[1] - Y[1], 2))
			
			
				# 最小距离
				def dist_min(Ci, Cj):
				    return min(dist(X, Y) for X in Ci for Y in Cj)
				
				
				# 最大距离
				def dist_max(Ci, Cj):
				    return max(dist(X, Y) for X in Ci for Y in Cj)
				
				
				# 平均距离
				def dis_avg(Ci, Cj):
				    return sum(dist(X, Y) for X in Ci for Y in Cj) / (len(Ci) * len(Cj))
				
			
				# 找到最小距离的下标
				def find_Mindis(M):
				    min = 1000
				    x = 0
				    y = 0
				    q = len(M)
				    for i in range(len(M)):
				        for j in range(len(M[i])):
				            if i != j and M[i][j] < min:
				                min = M[i][j]
				                x = i
				                y = j
				
				    # 返回距离矩阵中最小距离的下标
				    return (x, y, min)
			
			
				# 算法:
				def AGNES(dataSet, k):
				    # 第一步: 先对仅含一个样本的初始聚类簇和相应的距离矩阵进行初始化
				    C = []
				    M = []
				    for ci in dataSet:
				        # 先构造一维数组
				        c = [];
				        c.append(ci)
				        # 再将一维数组加入到数组中, 便构成了二维数组.
				        C.append(ci)
				    # 以下的二维数组构造同上
				    for ci in dataSet:
				        Mi = []
				        for cj in dataSet:
				            Mi.append(dist(ci, cj))
				        M.append(Mi)
				
				    # 第二步: 合并最近的聚类簇，并对合并得到的聚类簇的距离矩阵进行更新, 上述过程不断重复，直至达到预设的聚类簇数
				    # 设置当前聚类簇个数
				    q = len(dataSet)
				    while q > k:
				        # 找出距离最近的两个聚类簇
				        x, y, min = find_Mindis(M)
				        # 合并最近的聚类簇 C[x].extend(C[y]) C.remove(C[y])
				        # axis 指定拼接的方向，默认axis = 0（逐行拼接）（纵向的拼接沿着axis= 1方向）
				        C[x] = np.append(C[x], C[y], axis=0)
				        C = np.delete(C, y, axis=0)
				        # 对合并得到的聚类簇的距离矩阵进行更新
				        M = []
				        for ci in C:
				            Mi = []
				            for cj in C:
				                Mi.append(dist(ci, cj))
				            M.append(Mi)
				        q = q - 1
				
				    return C
			
			
				# 画图
				def draw(C):
				    # 颜色: r-red y-yellow g-green k-black m-magenta
				    colValue = ['r', 'y', 'g', 'b', 'c', 'k', 'm']
				    markerValue = ['D', 'd', '+', '*', 'x', '^', 's']
				    coo_X = []  # x坐标列表
				    coo_Y = []  # y坐标列表
				    # 坐标转换
				    for i in range(len(C)):
				        m = int((len(C[i]) / 2))  # 计算行数, 已知二维数组
				        C[i] = np.array(C[i]).reshape(m, 2)
				    # 保存 x, y 坐标, 再画图
				    for i in range(len(C)):
				        for j in range(len(C[i])):
				            coo_X.append(C[i][j][0])
				            coo_Y.append(C[i][j][1])
				        # 每次都要清除坐标列表, 不然颜色会重叠
				        pl.scatter(coo_X, coo_Y, s=40, c=colValue[i % len(colValue)], marker=markerValue[i % len(markerValue)])
				        coo_X.clear()
				        coo_Y.clear()
				
				    pl.title('AGNES')
				    pl.xlabel('X-axis')
				    pl.ylabel('Y-axis')
				    pl.show()
				
				
				if __name__ == "__main__":
				    dataSet = loadData('test.txt')
				    k = 2  # k代表预设的最终聚类簇数
				    C = AGNES(dataSet, k)
				    draw(C)
