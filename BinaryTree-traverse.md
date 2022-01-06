---
title: 二叉树的遍历框架
categories:
  - 算法
  - 树
tags:
  - 二叉树
author:
  - Jungle
  - labuladong
date: 2021-12-13 10:50:34
---



# 递归改迭代



## 递归框架

> 前 中 后 -序 遍历

```java
void traverse(TreeNode root) {
    if (root == null) return;
    /* 前序遍历代码位置 */
    traverse(root.left);
    /* 中序遍历代码位置 */
    traverse(root.right);
    /* 后序遍历代码位置 */
}
```



---



## 迭代框架

> 如果想把递归改成迭代，直接把递归解法中前中后序对应位置的代码复制粘贴到迭代框架里，就可以直接运行，得到正确的结果。
>
> 理论上，所有递归算法都可以利用栈改成迭代的形式，因为计算机本质上就是借助栈来迭代地执行递归函数的。

```java
void traverse(TreeNode root) {
    while (...) {
        if (...) {
          /* 前序遍历代码位置 */
        }
        if (...) {
          /* 中序遍历代码位置 */
        }
        if (...) {
          /* 后序遍历代码位置 */
        }
    }
}
```

 

> 模拟系统的函数调用栈

```java
// 模拟系统的函数调用栈
Stack<TreeNode> stk = new Stack<>();

void traverse(TreeNode root) {
    if (root == null) return;
    // 函数开始时压入调用栈
    stk.push(root);
    traverse(root.left);
    traverse(root.right);
    // 函数结束时离开调用栈
    stk.pop();
}
```

简单说就是这样一个流程：

**1、拿到一个节点，就一路向左遍历（因为 `traverse(root.left)` 排在前面），把路上的节点都压到栈里**。

**2、往左走到头之后就开始退栈，看看栈顶节点的右指针，非空的话就重复第 1 步**。

写成迭代代码就是这样：

```java
private Stack<TreeNode> stk = new Stack<>();

public List<Integer> traverse(TreeNode root) {
    pushLeftBranch(root);
    
    while (!stk.isEmpty()) {
        TreeNode p = stk.pop();
        pushLeftBranch(p.right);
    }
}

// 左侧树枝一撸到底，都放入栈中
private void pushLeftBranch(TreeNode p) {
    while (p != null) {
        stk.push(p);
        p = p.left;
    }
}
```

上述代码虽然已经可以模拟出递归函数的运行过程，不过还没有找到递归代码中的前中后序代码位置，所以需要进一步修改。



## [迭代代码框架](https://labuladong.github.io/algo/2/18/32/#迭代代码框架)

想在迭代代码中体现前中后序遍历，关键点在哪里？

**当我从栈中拿出一个节点 `p`，我应该想办法搞清楚这个节点 `p` 左右子树的遍历情况**。

- 如果 `p` 的左右子树都没有被遍历，那么现在对 `p` 进行操作就属于前序遍历代码。

- 如果 `p` 的左子树被遍历过了，而右子树没有被遍历过，那么现在对 `p` 进行操作就属于中序遍历代码。

- 如果 `p` 的左右子树都被遍历过了，那么现在对 `p` 进行操作就属于后序遍历代码。

上述逻辑写成伪码如下：

```java
private Stack<TreeNode> stk = new Stack<>();

public List<Integer> traverse(TreeNode root) {
    pushLeftBranch(root);
    
    while (!stk.isEmpty()) {
        TreeNode p = stk.peek();
        
        if (p 的左子树被遍历完了) {
            /*******************/
            /** 中序遍历代码位置 **/
            /*******************/
            // 去遍历 p 的右子树
            pushLeftBranch(p.right);
        }

        if (p 的右子树被遍历完了) {
            /*******************/
            /** 后序遍历代码位置 **/
            /*******************/
            // 以 p 为根的树遍历完了，出栈
            stk.pop();
        }
    }
}

private void pushLeftBranch(TreeNode p) {
    while (p != null) {
        /*******************/
        /** 前序遍历代码位置 **/
        /*******************/
        stk.push(p);
        p = p.left;
    }
}
```



> 有刚才的铺垫，这段代码应该是不难理解的，关键是如何判断 `p` 的左右子树到底被遍历过没有呢？

其实很简单，我们只需要维护一个 `visited` 指针，指向「上一次遍历完成的根节点」，就可以判断 `p` 的左右子树遍历情况了

**下面是迭代遍历二叉树的完整代码框架**：

```java
// 模拟函数调用栈
private Stack<TreeNode> stk = new Stack<>();

// 左侧树枝一撸到底
private void pushLeftBranch(TreeNode p) {
    while (p != null) {
        /*******************/
        /** 前序遍历代码位置 **/
        /*******************/
        stk.push(p);
        p = p.left;
    }
}

public List<Integer> traverse(TreeNode root) {
    // 指向上一次遍历完的子树根节点
    TreeNode visited = new TreeNode(-1);
    // 开始遍历整棵树
    pushLeftBranch(root);
    
    while (!stk.isEmpty()) {
        TreeNode p = stk.peek();
        
        // p 的左子树被遍历完了，且右子树没有被遍历过
        if ((p.left == null || p.left == visited) 
          && p.right != visited) {
            /*******************/
            /** 中序遍历代码位置 **/
            /*******************/
            // 去遍历 p 的右子树
            pushLeftBranch(p.right);
        }
        // p 的右子树被遍历完了
        if (p.right == null || p.right == visited) {
            /*******************/
            /** 后序遍历代码位置 **/
            /*******************/
            // 以 p 为根的子树被遍历完了，出栈
            // visited 指针指向 p
            visited = stk.pop();
        }
    }
}
```

代码中最有技巧性的是这个 `visited` 指针，它记录最近一次遍历完的子树根节点（最近一次 `pop` 出栈的节点），我们可以根据对比 `p` 的左右指针和 `visited` 是否相同来判断节点 `p` 的左右子树是否被遍历过，进而分离出前中后序的代码位置。

> `visited` 指针初始化指向一个新 new 出来的二叉树节点，相当于一个特殊值，目的是避免和输入二叉树中的节点重复。

---



### 前序遍历

```java
private Stack<TreeNode> stk = new Stack<>();

public List<Integer> preorderTraversal(TreeNode root) {
    // 记录后序遍历的结果
    List<Integer> preorder = new ArrayList<>();
    TreeNode visited = new TreeNode(-1);

    pushLeftBranch(root);
    while (!stk.isEmpty()) {
        TreeNode p = stk.peek();

        if ((p.left == null || p.left == visited) 
          && p.right != visited) {
            pushLeftBranch(p.right);
        }

        if (p.right == null || p.right == visited) {
            visited = stk.pop();
        }
    }

    return preorder;
}

private void pushLeftBranch(TreeNode p) {
    while (p != null) {
        // 前序遍历代码位置
        preorder.add(p.val);
        stk.push(p);
        p = p.left;
    }
}

```



---

### 中序遍历

```java
private Stack<TreeNode> stk = new Stack<>();

public List<Integer> midorderTraversal(TreeNode root) {
    // 记录后序遍历的结果
    List<Integer> midorder = new ArrayList<>();
    TreeNode visited = new TreeNode(-1);

    pushLeftBranch(root);
    while (!stk.isEmpty()) {
        TreeNode p = stk.peek();

        if ((p.left == null || p.left == visited) 
          && p.right != visited) {
            // 中序遍历代码位置
            midorder.add(p.val);
            pushLeftBranch(p.right);
        }

        if (p.right == null || p.right == visited) {
            visited = stk.pop();
        }
    }

    return midorder;
}

private void pushLeftBranch(TreeNode p) {
    while (p != null) {
        stk.push(p);
        p = p.left;
    }
}

```



---



### 后序遍历:

**只需把递归算法中的前中后序位置的代码复制粘贴到上述框架的对应位置，就可以把任意递归的二叉树算法改写成迭代形式了**。

比如，让你返回二叉树后序遍历的结果，你就可以这样写：

```java
private Stack<TreeNode> stk = new Stack<>();

public List<Integer> postorderTraversal(TreeNode root) {
    // 记录后序遍历的结果
    List<Integer> postorder = new ArrayList<>();
    TreeNode visited = new TreeNode(-1);

    pushLeftBranch(root);
    while (!stk.isEmpty()) {
        TreeNode p = stk.peek();

        if ((p.left == null || p.left == visited) 
          && p.right != visited) {
            pushLeftBranch(p.right);
        }

        if (p.right == null || p.right == visited) {
            // 后序遍历代码位置
            postorder.add(p.val);
            visited = stk.pop();
        }
    }

    return postorder;
}

private void pushLeftBranch(TreeNode p) {
    while (p != null) {
        stk.push(p);
        p = p.left;
    }
}
```

当然，任何一个二叉树的算法，如果你想把递归改成迭代，都可以套用这个框架，只要把递归的前中后序位置的代码对应过来就行了。

迭代解法到这里就搞定了，**不过我还是想强调，除了 BFS 层级遍历之外，二叉树的题目还是用递归的方式来做，因为递归是最符合二叉树结构特点的**。

说到底，这个迭代解法就是在用栈模拟递归调用，所以对照着递归解法，应该不难理解和记忆。



---

















