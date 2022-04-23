---
layout: post
title: "Leetcode Graph"
date: 2022-04-23
---

# [Leetcode 210](https://leetcode-cn.com/problems/course-schedule-ii/solution/ke-cheng-biao-ii-by-leetcode-solution/)
## 1. 题目要求
现在你总共有 n 门课 0 ~ n-1。想要学习课程 0，要先完成课程 1，用 [0,1] 表示。给定课程总量以及它们的先决条件，返回学完所有课程的顺序（返回一种即可），如果不可能完成所有课程，返回空数组。

## 2. 解法
- 生成图
  - 点 -> 联通点的LIST
- 维护一个ARRAY记录点的访问状态：无访问、访问中（防止DFS递归成环）、结束
- **DFS解法**
  - 从每个为访问的点出发，开始DFS，逐个将DFS点压入stack（golang要用array凑活）
- **BFS解法**
  - 也要先生成图
  - 要维护一个INT array用于记录入度（被多少点连通）
  - 遍历入度ARRAY，记录入度为0的点（作为起点）
  - 开始遍历起点，找寻它的partner~ 找到后，partner入度减一
  - 减到0时，就可以压入queue作为结果输出（golang要用array凑活）

```
```
LeetCode 743. 网络延迟时间
弗洛伊德算法 Floyd-Warshall Algorithm
迪杰斯特拉算法 Dijkstra Algorithm
Kruskal算法
并查集 Disjoint-set
LeetCode 684. 冗余连接
LeetCode 1135. 最低成本联通所有城市
Prim算法
二分图 Bipartite graph
LeetCode 785. 判断二分图
https://www.paincker.com/graph-theory/

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

