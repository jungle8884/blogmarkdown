---
title: k_means
categories:
  - 机器学习
  - 分类算法
tags:
  - Python
  - 划分聚类
date: 2020-02-27 
author: jungle

---
**k-means算法代码如下:**

**源代码: https://github.com/jungle8884/Kmeans**

		"""
		k = 2, n = 8
		数据:
		1	1
		2	1
		1	2
		2	2
		4	3
		5	3
		4	4
		5	4
		第一次绘图时:
		 变化之前调用showCluster(data, k, centroids, clusterAssment), 
		由于markIndex = int(clusterAssment[i, 0]) 等于0, 所以全是红色;
		变化之后再调用showCluster(data, k, centroids, clusterAssment),
		clusterAssment[i, :] = minIndex, minDist ** 2 更新, 所以一簇为红圈, 另一簇为蓝色正方形;
		其中, 质心用黑色*表示.
		以此类推, 获得最后图像
		"""
		import numpy as np
		import matplotlib.pyplot as plt


​		
​		# 加载数据
​		def loadData(filename):
​		    data = np.loadtxt(filename, delimiter='\t', encoding="UTF-8-sig")
​		    return data


​		
​		# 计算欧式距离
​		def euclideanDistances(X, Y):
​		    # X 代表坐标(x1, x2)   Y 代表坐标(y1, y2)
​		    return np.sqrt(np.sum((X - Y) ** 2))


​		
​		# 构建k个随机质心 k=2
​		def randCent(data, k):
​		    m, n = data.shape  # (8, 2) 数组的形式，对于矩阵，m 行 n 列,  列数表示维数
​		    centroids = np.zeros((k, n), dtype=np.int)  # (k=2, n=2) k 行 n 列, 列数与样本的列数一致
​		    for i in range(k):
​		        index = int(np.random.uniform(0, m))    # 随机生成序号 0~7  uniform---[low, high)
​		        centroids[i, :] = data[index, :]  # [index, :]表示第index行所有元素
​		    return centroids


​		
​		def kMeans(data, k):
​		    m = np.shape(data)[0]  # shape=(8,2) 表示8行2列, [0]此处获取行数
​		    clusterChange = True   # 若簇发生了改变为True
​		    # 第一列存样本属于哪一簇
​		    # 第二列存样本的到簇的中心点的误差
​		    # mat 代表 matrix 矩阵 [m 行 2 列], 行号0~7代表样本序号
​		    clusterAssment = np.mat(np.zeros((m, 2)))
​		
		    # 第1步 初始化centroids质心
		    centroids = randCent(data, k)   # 随机生成k=2个质心
		    while clusterChange:  # 若质心发生了改变就继续执行
		        clusterChange = False
		        # 显示质心变化之前---看过程用
		        # showCluster(data, k, centroids, clusterAssment)
		        # 遍历所有样本 data[0~7, :] m==8
		        # 第1轮的质心是在样本中的, 并没有排除, 因此会有两次样本自身的计算, 但并不影响结果
		        for i in range(m):
		            # 每一行都要找最小距离
		            minDist = 100000.0
		            minIndex = -1
		
		            # 遍历所有的质心 centroids[0~1, :]  k==2
		            # 第2步 找出最近的质心, 索引j保存到minIndex
		            for j in range(k):
		                # 计算该样本到质心的欧式距离
		                distance = euclideanDistances(centroids[j, :], data[i, :])
		                if distance < minDist:
		                    minDist = distance
		                    minIndex = j
		
		            # 第3步: 更新每一个样本所属的簇 i=minIndex
		            if clusterAssment[i, 0] != minIndex:  # 第一列元素值不等于最小质心索引值
		                clusterChange = True
		                clusterAssment[i, :] = minIndex, minDist ** 2  # 为第i行所有元素赋值
		
		        # 第4步: 更新质心, 每一轮更新一次
		        for j in range(k):
		            ''' nonzero(a) 返回数组a中非零元素的索引值所组成的数组。
		                np.mean() 二维矩阵，axis=0返回纵轴的平均值，axis=1返回横轴的平均值
		            clusterAssment[:, 0] 表示第一列数值---所属的簇(.A表示转换为 array)
		            clusterAssment[:, 0].A == j 表示属于第j簇
		            np.nonzero(clusterAssment[:, 0].A == j)[0] 取非零数组---由第j簇的索引值构成
		            np.mean(pointInCluster, axis=0) 对第j簇的值求平均值'''
		            pointInCluster = data[np.nonzero(clusterAssment[:, 0].A == j)[0]]  # 获取簇中所有点
		            centroids[j, :] = np.mean(pointInCluster, axis=0)  # 对簇求平均值求新的质心
		        # 质心变化之后---看过程用
		        # showCluster(data, k, centroids, clusterAssment)
		    # 完成后返回质心, 簇[所属质心, 误差]
		    return centroids, clusterAssment


​		
​		# 数据可视化
​		def showCluster(data, k, centroids, clusterAssment):
​		    m, n = data.shape   # (8, 2) 数组的形式，对于矩阵，m 行 n 列,  列数表示维数
​		    if n != 2:
​		        print("数据不是二维的")
​		        return 1
​		
		    # 红圆圈 蓝色正方形 黑色正三角-质心颜色
		    mark = ['or', 'sb', '*k', '*k']
		    if k > len(mark):
		        print("k值太大了")
		        return 1
		
		    # 绘制坐标轴, 标题
		    plt.title('k-means')
		    plt.xlabel('X-Axis')
		    plt.ylabel('Y-Axis')
		
		    # 绘制所有的样本
		    for i in range(m):
		        markIndex = int(clusterAssment[i, 0])
		        plt.plot(data[i, 0], data[i, 1], mark[markIndex])  # (x, y, 颜色---[0~1])
		
		    # 绘制质心
		    for i in range(k):
		        plt.plot(centroids[i, 0], centroids[i, 1], mark[i+2])  # 质心颜色为: mark[2~3]
		
		    plt.show()


​		
​		''' if __name__ == "__main__":
​		__name__ 是当前模块名，当模块被直接运行时模块名为 __main__ 
​		这句话的意思就是，当模块被直接运行时，以下代码块将被运行，
​		当模块是被导入时，代码块不被运行。'''
​		if __name__ == "__main__":
​		    data = loadData('test.txt')
​		    k = 2
​		    centroids, clusterAssment = kMeans(data, k)
​		    showCluster(data, k, centroids, clusterAssment)  # 最终图像

