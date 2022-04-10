---
layout: post
title: "Leetcode BFS & DFS"
date: 2022-04-20
---

BFS：https://juejin.cn/post/7052914822941245476

# [Leetcode 130](https://leetcode-cn.com/problems/surrounded-regions/)
## 1. 题目要求
给你一个 m x n 的矩阵 board ，由若干字符 'X' 和 'O' ，找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。
 

示例 1：
输入：
board = 
[

    ["X","X","X","X"],
    ["X","O","O","X"],
    ["X","X","O","X"],
    ["X","O","X","X"]
]
输出：
[

    ["X","X","X","X"],
    ["X","X","X","X"],
    ["X","X","X","X"],
    ["X","O","X","X"]
]

解释：被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

示例 2：
输入：board = [["X"]]
输出：[["X"]]


## 2. 解法

```
func solve(board [][]byte)  {

}
```

# [Leetcode 200](https://leetcode-cn.com/problems/number-of-islands/)
## 1. 题目要求
给你一个大小为 m x n 的整数矩阵 grid ，表示一个网格。另给你三个整数 row、col 和 color 。网格中的每个值表示该位置处的网格块的颜色。

两个网格块属于同一 连通分量 需满足下述全部条件：

两个网格块颜色相同
在上、下、左、右任意一个方向上相邻
连通分量的边界 是指连通分量中满足下述条件之一的所有网格块：

在上、下、左、右任意一个方向上与不属于同一连通分量的网格块相邻
在网格的边界上（第一行/列或最后一行/列）
请你使用指定颜色 color 为所有包含网格块 grid[row][col] 的 连通分量的边界 进行着色，并返回最终的网格 grid 。

 

示例 1：
输入：grid = 
[
    [1,1],
    [1,2]
]
row = 0, col = 0, color = 3
输出：
[
    [3,3],
    [3,2]
]

示例 2：
输入：grid = 
[
    [1,2,2],
    [2,3,2]
],
row = 0, col = 1, color = 3
输出：
[
    [1,3,3],
    [2,3,3]
]


## 2. 解法

```
func colorBorder(grid [][]int, row int, col int, color int) [][]int {

}
```



# [Leetcode 695](https://leetcode-cn.com/problems/max-area-of-island/)
## 1. 题目要求
给你一个大小为 m x n 的二进制矩阵 grid 。

岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在 水平或者竖直的四个方向上 相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

岛屿的面积是岛上值为 1 的单元格的数目。

计算并返回 grid 中最大的岛屿面积。如果没有岛屿，则返回面积为 0 。

![image](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

输入：grid =
 [

     [0,0,1,0,0,0,0,1,0,0,0,0,0],
     [0,0,0,0,0,0,0,1,1,1,0,0,0],
     [0,1,1,0,1,0,0,0,0,0,0,0,0],
     [0,1,0,0,1,1,0,0,1,0,1,0,0],
     [0,1,0,0,1,1,0,0,1,1,1,0,0],
     [0,0,0,0,0,0,0,0,0,0,1,0,0],
     [0,0,0,0,0,0,0,1,1,1,0,0,0],
     [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
输出：6
解释：答案不应该是 11 ，因为岛屿只能包含水平或垂直这四个方向上的 1 。


## 2. 解法

```
func maxAreaOfIsland(grid [][]int) int {

}
```

# [Leetcode 1034]([(https://leetcode-cn.com/problems/number-of-islands)/](https://leetcode-cn.com/problems/coloring-a-border/))
## 1. 题目要求
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。
岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。
此外，你可以假设该网格的四条边均被水包围。

示例 1：
输入：
grid =
[
    
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]

输出：1


## 2. 解法

```
```

# [Leetcode 102](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
## 1. 题目要求
给你二叉树的根节点 root ，返回其节点值的 层序遍历 。 （即逐层地，从左到右访问所有节点）。


## 2. 解法

```
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func levelOrder(root *TreeNode) [][]int {

}
```

# [Leetcode 542](https://leetcode-cn.com/problems/01-matrix/)
## 1. 题目要求
给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。
输入：
mat = 
[
    [0,0,0],
    [0,1,0],
    [0,0,0]
]
输出：
[
    [0,0,0],
    [0,1,0],
    [0,0,0]
]


## 2. 解法

```
func updateMatrix(mat [][]int) [][]int {

}
```

# [Leetcode ]()
## 1. 题目要求


## 2. 解法

```
```