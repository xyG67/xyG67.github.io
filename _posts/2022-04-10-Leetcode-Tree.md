---
layout: post
title: "Leetcode Tree"
date: 2022-04-10
---

https://darktiantian.github.io/LeetCode%E7%AE%97%E6%B3%95%E9%A2%98%E6%95%B4%E7%90%86%EF%BC%88%E4%BA%8C%E5%8F%89%E6%A0%91%E7%AF%87%EF%BC%89Tree/

# [Leetcode 144]()
## 1. 题目要求
preorder traversal

add nodes to List<Integer>

*定义：从左到右，紧着左边加

可以用iteration也可以用stack实现

## 2. 解法

```
public List<Integer> preorderTraversal(TreeNode root) {

        List<Integer> res = new LinkedList<Integer>();
        if(root == null) return res;
        
        res.add(root.val);
        res.addAll(preorderTraversal(root.left));
        res.addAll(preorderTraversal(root.right));
        
        return res;
    }
```



# [Leetcode 222]()
## 1. 题目要求
给一个complete binary tree（所有点都尽可能靠左），统计有几个点

## 2. Code

*1<<n 为2^n，而2^n-1刚好是左节点、右节点都有的二叉树的点的总数。

因为所有点都尽可能靠左，所以只需计算左右深度是否相同

如果相同就愉快的带入上述公式，如果不是就将左子树和右子树分开，recursive

Java

```
public int countNodes(TreeNode root) {
        
        int count_L = countLeft(root);
        int count_R = countRight(root);
        
        if(count_L == count_R) 
            return (1<<count_L)-1;
        else
            return    1+countNodes(root.left) + countNodes(root.right);
    }
    
    public int countLeft(TreeNode root) {
        int count = 0;
        while(root!=null) {
            root = root.left;
            count++;
        }
        return count;
    }
    
    public int countRight(TreeNode root) {
        int count = 0;
        while(root!=null) {
            root = root.right;
            count++;
        }
        return count;
    }
}
```



# [Leetcode ]()
## 1. 题目要求


## 2. 解法

```
```




# [Leetcode ]()
## 1. 题目要求


## 2. 解法

```
```




# [Leetcode ]()
## 1. 题目要求


## 2. 解法

```
```




# [Leetcode ]()
## 1. 题目要求


## 2. 解法

```
```


