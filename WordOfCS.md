 ---
title: 计算机英语词汇
date: 2019/8/10
author: Jungle
categories:
- 英语
tags:
- 词汇
---

# 分布式数据库系统及原理 #

- Transaction	事务
- Concurrent	并发
- Parallel		并行
- DDBS			分布式数据库系统
	- Distributed Database System
	- 地理上分散而逻辑上集中的数据库系统
	- DDBS = DDB + DDBMS
	- 分布式数据库系统 = 分布式数据库 + 分布式数据库管理系统
- DB			数据库
- DBMS			数据库管理系统
- DB + DBMS = DBS	
- 数据库 + 数据库管理系统 = 数据库系统
- SQL			结构化查询语言
- node			节点
- site			场地
- p				处理机
- M				内存
- D				磁盘
- Shared-Memery Architecture	共享内存系统
- Shared-Disk   Architecture	共享磁盘系统
- Shared-Nothing Architecture	无共享系统
- union			并				∪
- intersect		交				∩
- difference	差				-
- product		乘				×
- divide		除				÷
- selcect		选择				σ
- project		投影				π
- join			连接				⋈
- schema		模式
- wrapper		包装器
- mediator		协调器
- MDBS			多数据库系统[multi-Database System]
- LDBS   		局部数据库系统
- FDBS			联邦数据库系统
- P2PDBS		对等数据库系统
- P2P			peer-to-Peer	对等计算模型
- fragment		片段
- fragmentation	分片
- allocation	分配
- partition		分割
- replication	复制
- GDB			Global DB		全局数据库
- FDB			Fragment DB		片段数据库
- PDB			Physical DB		物理数据库

----------

# Maching Learning 基本术语 #

## 第一章 ##

- model 模型【learning algorithm 学习算法】
- data set		数据集
	- 记录的集合
	- 每条记录是关于一个事件或对象的描述
- instance		示例
	- 【或 sample 样本】
	- 【一个示例就是一条记录】
- attribute		属性
	- 或 [反应事件或对象在某方面的表现或性质的事项.]
- feature		特征
- attribute value	属性值
- attribute space	属性空间
	- 属性张成的空间
- sample space      样本空间
- feature vector	特征向量
- dimensionality	维数
- traning data		训练数据
- traning sample	训练样本
	- 或
- traning instance	训练示例
- traning set		训练集
	- 训练样本组成的集合
- hypothesis		假设
- ground-truth		真相【潜在规律】
- learner			学习器【模型】
	- 有时将模型称为 "学习器", 可看作学习算法在给定数据和参数空间上的实列话.
- prediction		预测
- label				标记
- example			样例【拥有了标记信息的示例】
	- (Xi,yi) 表示第i个样例
- label	space		标记空间【或 输出空间】
	- Xi 是一个示例
	- yi 是示例Xi的标记
	- Y 是所有标记yi的集合
- classification	分类
	- 预测的是离散值
- regression		回归
	- 预测的是连续值
- binary classification	二分类
	- 只涉及两个类别
	-  positive	class	正类
	-  negative class	反类
-  multi-class classification	多分类
	-  涉及多个类别
- testing			测试【进行预测的过程】
- testing sample	测试样本【被预测的样本】
- clustering  		聚类【将训练集中的西瓜分为若干组】
- cluster			簇【每一组称为一个簇】
- supervised learning	监督学习
	- 分类
	- 回归
- unsupervised learning 无监督学习【事先不知道，而且学习过程中使用的训练样本通常不拥有标记信息】
	- 聚类
- generalization	泛化【学的模型适用于新样本的能力】
- distribution		分布
- independent and identically distributed 	独立同分布
	- abbreviated as i.i.d
- induction			归纳
	- 特殊到一般的 "泛化"[generalization] 过程
	- 从具体的事实归结出一般性规律
	- 从样例中学习显然是一个归纳的过程
- deduction			演绎
	- 一般到特殊的 "特化"[specialization] 过程
	- 从基础原理推演处具体状况
	- 基于一组公理和推导规则推导出与之相恰的定理
- inductive learning	归纳学习
	- 从样例中学习		[广义上的]
	- 从训练数据中学得概念[狭义上的, 又称概念学习]
- concept		概念
- fit			匹配
- version space 版本空间
	- 【存在着一个与训练集一致的“假设集合”，称为版本空间】
- inductive bias	归纳偏好
	- 【机器学习算法在学习过程中对某种类型假设的偏好】
	- 任何一个有效的机器学习算法必有其归纳偏好，
	- 否则它将被假设空间中在训练集中看似“等效”的假设所迷惑，而无法产生确定的学习结果。
	- 【算法的归纳偏好是否与问题本身匹配，大多数时候决定了算法能否取得好的性能。】
- Occam's razor 奥卡姆剃刀 【一种常用的, 自然科学研究中最基本的原则】
	-  若有多个假设与观察一致, 则选择最简单的那个.
- NFL	No Free Lunch Theorem "没有免费的午餐"定理



## 第二章 ##
1. error rate 错误率
2. accuracy 精度
3. error 误差
4. 在训练集上的误差:	
	1. training error 训练误差
	2. 或 empirical error 经验误差
5. 在新样本上的误差: generalization error 泛化误差
6. overfitting 过拟合
	1. 把训练样本自身的一些特点当作了所有潜在样本都会具有的一般性质, 导致泛化性能下降.
	2. 学习能力过于强大, 以至于把训练样本不一般的特性都学到了.
	3. 过拟合是机器学习的关键障碍.
	4. 过拟合是无法彻底避免的.
7. underfitting 欠拟合[学习能力低下造成, 比较容易克服.]
8. model selection 模型选择
9. testing set 测试集
10. testing error 测试误差
11. 评估方法
	1. hold-out 流出法
	2. cross validation 交叉验证法 [又称: K-fold corss validation k折交叉验证]
	3. bootstrapping 自助法
12. sampling 采样
13. stratified sampling 分层采样 [保留类别比例的分层方式]
14. fidelity 保真性
15. out-of-bag estimate 包外估计
16. parameter tuning 调参
17. performance measure 性能度量
18. mean squared error 均方误差
19. precision 查准率
20. recall 查全率
21. true positive 真正例
22. false positive 假正例
23. true negative 真反例
24. false negative 假反例
25. confusion matrix 混淆矩阵
26. BEP: Break-Even Point 平衡点
27. harmonic mean 调和平均数
28. macro 宏 [macro-P 宏查准率; macro-R 宏查全率; macro-F1]
29. micro 微 [micro-P 宏查准率; micro-R 宏查全率; micro-F1]
30. F1: 基于查准率和查全率的调和平均定义的. 
31. Fβ: 则是加权调和平均; 第32页.
32. threshold 分类阈值
33. cut point 截断点
34. ROC: Receiver Operating Characteristic "受试者工作者特征"曲线

----------
# 深度学习 #
1. 损失函数	 	loss function
2. 均方误差 		mean squared error
3. 激活函数		activation function
4. sigmoid 函数		sigmoid function
5. 结构化数据	structured data
6. Numpy		Numerical Python 是Python科学计算的基础包
7. Pandas		Python data analysis (Python数据分析)
8. 交叉熵误差	cross entropy error
9. 小批量		mini-batch
10. 数值微分		numerical differentiation
11. 舍入误差		rounding error
12. 解析性		analytic
13. 鞍点			saddle point
14. 梯度法		gradient method
15. 学习率		learning rate
16. 随机梯度下降法		stochastic gradient descent
17. 计算图		computational graph
18. 正向传播		forward propagation
19. 反向传播		backward propagation
20. 链式法则		chain rule
21. 乘法层		MulLayer
22. 加法层		AddLayer
23. 梯度确认		gradient check
24. 最优化		optimization
25. 非均向		anisotropic
26. 权值衰减		weight decay
27. 直方图		histogram
28. 梯度消失		gradient vanishing

----------

# 计算机专业英语 #
- computer graphics											计算机图形学
- cmputational complexity theory							计算复杂性理论
- computability theory										可计算性理论
- computationally unsolvable and intractable problems		计算不可解和不可行问题
- scientific computing										科学计算
- artificial intelligence									人工智能
- machine learning											机器学习
- ACM [Asscociation for Computing Machinery]				美国计算机学会
- IEEE[Institute of Electrical and Electronic Engineers]	电气电子工程师协会
- algorithm and data structrues								算法和数据结构
- programming methodology and languages						程序设计方法与语言
- software engineering										软件工程
- computer elements and architecture						计算机组成与架构
- computer network and communication						计算机网络与通信
- database system											数据库系统
- parallel computation										并行计算
- distributed computation									分布式计算
- numerical and symbolic computation						数值和符号计算
- mathematical logic										数理逻辑
- automata theory											自动机理论
- number   theory											数论
- graphy   theory											图论
- type     theory											类型论
- category theory											范畴论
- computational geometry									计算几何
- quantum computing theory									量子计算理论
- cryptography												密码学
- digital  logic											数字逻辑
- microarchitecture											微架构
- multiprocessing											多处理
- domain theory												范畴论
- algebra													代数
- numerical analysis										数值分析
- computational physics										计算物理
- bioinformatics											生物信息学
- computer vision											计算机视觉
- image processing											图像处理
- pattern recognition										模式识别
- cognitive science											认知科学
- data mining												数据挖掘
- evolutionary computation									演化计算
- information retrieval										信息检索
- knowledge representation									知识表达
- nature language processing								自然语言处理
- robotics													机器人学
- human-computer interaction								人机交互
- operating systems											操作系统
- computer security											计算机安全
- ubiquitous computing										普适计算
- compiler design											编译器设计
- inherent													固有的
- disposed													有倾向性的, 愿意的
- pernicious												有害的
- evangelize												传道
- manifest													表明
- oligarchy													寡头政治
- unassailable												无懈可击的
- shill														诱饵, 托儿
- breed														引起
- sinister													阴险的
- vigorously												精力旺盛地
- allegation												主张, 断言
- divulge													泄露
- unscrupulous												不讲道德的
- cybersecurity												网络安全
- exposure													暴露
- underground economy										地下交易
- intellectual property										知识产权
- breach													缺口, 破坏
- trillion													万亿
- malicious													恶意的
- infection													感染
- merely													仅仅, 只不过
- SQL injection												SQL注入攻击
- victim													受害者
- cross-site scripting										跨站点脚本攻击
- rootkits													一种嵌入在操作系统中的恶意程序
- botnet													僵尸网络
- spam														垃圾邮件
- phishing													网络钓鱼攻击
- crack														破解
- software patch											软件补丁
- evade														逃避
- launch													发起
- social engineering										社交工程
- compromise												妥协
- penetration test											渗透测试
- fraudulent												欺骗性的
- firewalls													防火墙
- intrusion detection										入侵检测
- afterthought												事后的想法
- exploit													利用
- rigorous													严格的
- albeit													虽然, 即使
- software defect 											软件缺陷
- cloud computing											云计算
- trigger													触发


----------
