---
title: K个一组反转链表
categories:
  - 算法
  - 链表
tags:
  - 25
  - 链表
  - 迭代
author:
  - Jungle
date: 2021-11-19 
---

# [K个一组反转链表](https://labuladong.gitee.io/algo/2/17/18/)

> https://leetcode-cn.com/problems/reverse-nodes-in-k-group/
>
> <img src="LinkedList-25-reverseKGroup/image-20211119161151116.png" alt="image-20211119161151116" style="zoom:80%;margin:0px;" />

---



## [分析问题](https://labuladong.gitee.io/algo/2/17/18/#一分析问题)

链表是一种兼具递归和迭代性质的数据结构，认真思考一下可以发现**这个问题具有递归性质**。

什么叫递归性质？直接上图理解，比如说我们对这个链表调用 `reverseKGroup(head, 2)`，即以 2 个节点为一组反转链表：

<img src="LinkedList-25-reverseKGroup/image-20211119161424478.png" alt="image-20211119161424478" style="zoom:80%;margin:10px;" />

如果我设法把前 2 个节点反转，那么后面的那些节点怎么处理？后面的这些节点也是一条链表，而且规模（长度）比原来这条链表小，这就叫**子问题**。

<img src="LinkedList-25-reverseKGroup/image-20211119161831591.png" alt="image-20211119161831591" style="zoom:80%;margin:10px;" />

我们可以直接递归调用 `reverseKGroup(cur, 2)`，因为子问题和原问题的结构完全相同，这就是所谓的递归性质。

发现了递归性质，就可以得到大致的算法流程：

**1、先反转以 `head` 开头的 `k` 个元素**。

<img src="LinkedList-25-reverseKGroup/image-20211119161921030.png" alt="image-20211119161921030" style="zoom:80%;margin:10px;" />

**2、将第 `k + 1` 个元素作为 `head` 递归调用 `reverseKGroup` 函数**。

<img src="LinkedList-25-reverseKGroup/image-20211119162008388.png" alt="image-20211119162008388" style="zoom:80%;margin:10px;" />

**3、将上述两个过程的结果连接起来**。

<img src="LinkedList-25-reverseKGroup/image-20211119162208549.png" alt="image-20211119162208549" style="zoom:80%;margin:10px;" />

整体思路就是这样了，最后一点值得注意的是，递归函数都有个 base case，对于这个问题是什么呢？

题目说了，如果最后的元素不足 `k` 个，就保持不变。这就是 base case，待会会在代码里体现。

---



## 代码实现



### 反转整个链表

首先，我们要实现一个 `reverse` 函数反转一个区间之内的元素。

在此之前我们再简化一下，给定链表头结点，如何反转整个链表？

```java
// 反转以 a 为头结点的链表
ListNode reverse(ListNode a) {
    ListNode pre, cur, nxt;
    pre = null; cur = a; nxt = a;
    while (cur != null) {
        nxt = cur.next;
        // 逐个结点反转
        cur.next = pre;
        // 更新指针位置
        pre = cur;
        cur = nxt;
    }
    // 返回反转后的头结点
    return pre;
}
```



### 反转 `[a` 到 `b)` 之间的结点

「反转以 `a` 为头结点的链表」其实就是「反转 `a` 到 null 之间的结点」，那么如果让你「反转 `a` 到 `b` 之间的结点」，你会不会？

只要更改函数签名，并把上面的代码中 `null` 改成 `b` 即可：

```java
/** 反转区间 [a, b) 的元素，注意是左闭右开 */
ListNode reverse(ListNode a, ListNode b) {
    ListNode pre, cur, nxt;
    pre = null; cur = a; nxt = a;
    // while 终止的条件改一下就行了
    while (cur != b) {
        nxt = cur.next;
        cur.next = pre;
        pre = cur;
        cur = nxt;
    }
    // 返回反转后的头结点
    return pre;
}
```

---



### k个一组反转链表

现在我们迭代实现了反转部分链表的功能，接下来就按照之前的逻辑编写 `reverseKGroup` 函数即可：

```java
ListNode reverseKGroup(ListNode head, int k) {
    if (head == null) return null;
    // 区间 [a, b) 包含 k 个待反转元素
    ListNode a, b;
    a = b = head;
    for (int i = 0; i < k; i++) {
        // 不足 k 个，不需要反转，base case
        if (b == null) return head;
        b = b.next;
    }
    // 反转前 k 个元素
    ListNode newHead = reverse(a, b);
    // 递归反转后续链表并连接起来
    a.next = reverseKGroup(b, k);
    return newHead;
}
```

解释一下 `for` 循环之后的几句代码，注意 `reverse` 函数是反转区间 `[a, b)`，所以情形是这样的：

<img src="LinkedList-25-reverseKGroup/image-20211119163220573.png" alt="image-20211119163220573" style="zoom:80%;margin:10px;" />

递归部分就不展开了，整个函数递归完成之后就是这个结果，完全符合题意：

<img src="LinkedList-25-reverseKGroup/image-20211119163344779.png" alt="image-20211119163344779" style="zoom:80%;margin:10px;" />



**整体代码:**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {

    private ListNode reverse(ListNode a, ListNode b) {
        ListNode pre, cur, nxt;
        pre = null; cur = a; nxt = a;
        while (cur != b) {
            nxt = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nxt;
        }

        return pre;
    }

    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null) { return null; }
        ListNode a, b;
        a = b = head;
        for (int i = 0; i < k; i++) {
            if (b == null) { return head; }
            b = b.next;
        }

        ListNode newHead = reverse(a, b);
        a.next = reverseKGroup(b, k);

        return newHead;
    }
}
```



---









