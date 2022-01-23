---
title: 图的数据结构
categories:
  - 算法
  - 图
tags:
  - 图
author:
  - Jungle
  -  labuladong
date: 2021-12-31 10:41:29
---

# 图的逻辑结构

## 逻辑结构

> N叉树的逻辑结构

```java
/* 基本的 N 叉树节点 */
class TreeNode {
    int val;
    TreeNode[] children;
}
```



> 图的逻辑结构

```java
/* 图节点的逻辑结构 */
class Vertex {
    int id;
    Vertex[] neighbors;
}
```

**也就是说, 图本质上还是多叉树.**



> 邻接表和邻接矩阵
>
> - 邻接表很直观，我把每个节点 `x` 的邻居都存到一个列表里，然后把 `x` 和这个列表关联起来，这样就可以通过一个节点 `x` 找到它的所有相邻节点。
>- 邻接矩阵则是一个二维布尔数组，我们权且称为 `matrix`，如果节点 `x` 和 `y` 是相连的，那么就把 `matrix[x][y]` 设为 `true`（上图中绿色的方格代表 `true`）。如果想找节点 `x` 的邻居，去扫一圈 `matrix[x][..]` 就行了。

<img src="https://labuladong.github.io/algo/images/%e5%9b%be/0.jpg" alt="img" style="zoom:100%;" />



用邻接表和邻接矩阵的存储方式如下：

<img src="https://labuladong.github.io/algo/images/%e5%9b%be/2.jpeg" alt="img" style="zoom:67%;margin:a50%;" />

代码的表现形式:

```java
// 邻接表
// graph[x] 存储 x 的所有邻居节点
List<Integer>[] graph;

// 邻接矩阵
// matrix[x][y] 记录 x 是否有一条指向 y 的边
boolean[][] matrix;
```



更进一步, **有向加权图**的表现形式:

```java
// 邻接矩阵
// graph[x] 存储 x 的所有邻居节点以及对应的权重
List<int[]>[] graph;

// 邻接矩阵
// matrix[x][y] 记录 x 指向 y 的边的权重，0 表示不相邻
int[][] matrix;
```

如果是邻接表，我们不仅仅存储某个节点 `x` 的所有邻居节点，还存储 `x` 到每个邻居的权重，不就实现加权有向图了吗？

如果是邻接矩阵，`matrix[x][y]` 不再是布尔值，而是一个 int 值，0 表示没有连接，其他值表示权重，不就变成加权有向图了吗？



**无向图怎么实现**？也很简单，所谓的「无向」，是不是等同于「双向」？

如果连接无向图中的节点 `x` 和 `y`，把 `matrix[x][y]` 和 `matrix[y][x]` 都变成 `true` 不就行了；邻接表也是类似的操作，在 `x` 的邻居列表里添加 `y`，同时在 `y` 的邻居列表里添加 `x`。

把上面的技巧合起来，就变成了无向加权图……(代码同上, 特殊处理即可)

---



## 图的遍历

> 多叉树遍历框架

```java
/* 多叉树遍历框架 */
void traverse(TreeNode root) {
    if (root == null) return;

    for (TreeNode child : root.children) {
        traverse(child);
    }
}
```

图和多叉树最大的区别是，图是可能包含环的，你从图的某一个节点开始遍历，有可能走了一圈又回到这个节点。

所以，如果图包含环，遍历框架就要一个 `visited` 数组进行辅助：

```java
// 记录被遍历过的节点
boolean[] visited;
// 记录从起点到当前节点的路径
boolean[] onPath;

/* 图遍历框架 */
void traverse(Graph graph, int s) {
    if (visited[s]) return;
    // 经过节点 s，标记为已遍历
    visited[s] = true;
    // 做选择：标记节点 s 在路径上
    onPath[s] = true;
    for (int neighbor : graph.neighbors(s)) {
        traverse(graph, neighbor);
    }
    // 撤销选择：节点 s 离开路径
    onPath[s] = false;
}
```



注意 `visited` 数组和 `onPath` 数组的区别，因为二叉树算是特殊的图，所以用遍历二叉树的过程来理解下这两个数组的区别：

<img src="https://labuladong.gitee.io/algo/images/%e8%bf%ad%e4%bb%a3%e9%81%8d%e5%8e%86%e4%ba%8c%e5%8f%89%e6%a0%91/1.gif" alt="img" style="zoom: 67%;" />

**上述 GIF 描述了递归遍历二叉树的过程，在 `visited` 中被标记为 true 的节点用灰色表示，在 `onPath` 中被标记为 true 的节点用绿色表示**，这下你可以理解它们二者的区别了吧。

来源: [图论基础 :: labuladong的算法小抄 (gitee.io)](https://labuladong.gitee.io/algo/2/19/35/)

---



## 上手题目

> 题目: [797. 所有可能的路径 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

给你一个有 `n` 个节点的 **有向无环图（DAG）**，请你找出所有从节点 `0` 到节点 `n-1` 的路径并输出（**不要求按特定顺序**）

二维数组的第 `i` 个数组中的单元都表示有向图中 `i` 号节点所能到达的下一些节点，空就是没有下一个结点了。

译者注：有向图是有方向的，即规定了 a→b 你就不能从 b→a 。



**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg)

```
输入：graph = [[1,2],[3],[3],[]]
输出：[[0,1,3],[0,2,3]]
解释：有两条路径 0 -> 1 -> 3 和 0 -> 2 -> 3
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg)

```
输入：graph = [[4,3,1],[3,2,4],[3],[4],[]]
输出：[[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
```

**示例 3：**

```
输入：graph = [[1],[]]
输出：[[0,1]]
```

**示例 4：**

```
输入：graph = [[1,2,3],[2],[3],[]]
输出：[[0,1,2,3],[0,2,3],[0,3]]
```

**示例 5：**

```
输入：graph = [[1,3],[2],[3],[]]
输出：[[0,1,2,3],[0,3]]
```



**提示：**

- `n == graph.length`
- `2 <= n <= 15`
- `0 <= graph[i][j] < n`
- `graph[i][j] != i`（即，不存在自环）
- `graph[i]` 中的所有元素 **互不相同**
- 保证输入为 **有向无环图（DAG）**



**解法很简单，以 `0` 为起点遍历图，同时记录遍历过的路径，当遍历到终点时将路径记录下来即可**。

既然输入的图是无环的，我们就不需要 `visited` 数组辅助了，直接套用图的遍历框架：

```java
//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    // 记录所有路径
    List<List<Integer>> res = new LinkedList<>();

    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        LinkedList<Integer> path = new LinkedList<>();
        traverse(graph, 0, path);
        return res;
    }

    void traverse(int[][] graph, int s, LinkedList<Integer> path) {
        // 添加结点 s 到路径
        path.addLast(s);

        int n = graph.length;
        // 到达终点
        if (s == n-1) {
            res.add(new LinkedList<>(path));
            path.removeLast();
            return;
        }

        for (int v : graph[s]) {
            traverse(graph, v, path);
        }

        // 从路径中移出结点 s
        path.removeLast();
    }
}
//leetcode submit region end(Prohibit modification and deletion)
```

**注意 Java 的语言特性，向 `res` 中添加 `path` 时需要拷贝一个新的列表，否则最终 `res` 中的列表都是空的。**

---











