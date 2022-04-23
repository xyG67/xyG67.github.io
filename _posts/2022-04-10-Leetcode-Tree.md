---
layout: post
title: "Leetcode Tree"
date: 2022-04-10
---

https://darktiantian.github.io/LeetCode%E7%AE%97%E6%B3%95%E9%A2%98%E6%95%B4%E7%90%86%EF%BC%88%E4%BA%8C%E5%8F%89%E6%A0%91%E7%AF%87%EF%BC%89Tree/

Leetcode 515 在每个树行中找最大值,【层序遍历即可】

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



# [Easy Leetcode 101 对称树](https://leetcode-cn.com/problems/symmetric-tree/)
## 1. 题目要求
给你一个二叉树的根节点 root ， 检查它是否轴对称。


## 2. 解法

```
func isSymmetric(root *TreeNode) bool {
    return checkSymmetric(root, root)
}

func checkSymmetric(left, right *TreeNode) bool {
    if left == nil && right != nil {
        return false
    }
    if right == nil && left != nil {
        return false
    }
    if right == nil && left == nil {
        return true
    }
    if right.Val != left.Val {
        return false
    }

    return checkSymmetric(left.Left, right.Right) && checkSymmetric(left.Right, right.Left)
}
```




# [Easy Leetcode 111](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
## 1. 题目要求
给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明：叶子节点是指没有子节点的节点。(好坑)

## 2. 解法

```
func minDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    left := dfs(root.Left)
    right := dfs(root.Right)

    min := left
    if right < left {
        min = right
    }
    if root.Left == nil && root.Right == nil {
        return min + 1
    }
    if root.Left == nil || root.Right == nil {
        return right + left + 1
    }
    return min + 1
}
```

# [Medium Leetcode 103](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
## 1. 题目要求
给你二叉树的根节点 root ，返回其节点值的 锯齿形层序遍历 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。


## 2. 解法

```
func zigzagLevelOrder(root *TreeNode) [][]int {
    
    queue := NewQueue()
    ret := [][]int{}
    floor := 0
    if root == nil {
        return ret
    }

    queue.push(root)

    for queue.len() > 0 {
        length := queue.len()
        currArr := []int{}
        for i:=0; i<length;i++ {
            node, _ := queue.pop()
            treeNode, _ := node.(*TreeNode)

            if floor % 2 == 0 {
                currArr = append(currArr, treeNode.Val)
            } else {
                currArr = append([]int{treeNode.Val}, currArr...)
            }
            if treeNode.Left != nil {
                queue.push(treeNode.Left)
            }
            if treeNode.Right != nil {
                 queue.push(treeNode.Right)
            }
        }            
        floor++  // 注意一下
        ret = append(ret, currArr)
    }
    return ret
}

type Queue struct {
    queue []interface{}
}

func NewQueue() *Queue {
    return &Queue{queue: []interface{}{}}
}

func (q *Queue) pop() (interface{}, error) {
    if len(q.queue) == 0 {
        return 0, errors.New("empty queue")
    }
    ret := q.queue[0]
    q.queue = q.queue[1:]
    return ret, nil
}

func (q *Queue) push(val interface{}) {
    q.queue = append(q.queue, val)
}

func (q *Queue) len() int {
    return len(q.queue)
}

```

# [Medium Leetcode 105 Build Tree](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
## 1. 题目要求
给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。

- 没有重复元素

## 2. 解法
前序遍历，根节点 + 左子树 + 右子树
中序遍历，左子树 + 根节点 + 右子树

- 定位根节点
- 构造ROOT
- left = 前序左子树 + 中序左子树
- right = 前序右子树 + 中序右子树


```
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 {
        return nil
    }
    root := &TreeNode{preorder[0], nil, nil}
    i := 0
    for ; i < len(inorder); i++ {
        if inorder[i] == preorder[0] {
            break
        }
    }
    root.Left = buildTree(preorder[1:len(inorder[:i])+1], inorder[:i])
    root.Right = buildTree(preorder[len(inorder[:i])+1:], inorder[i+1:])
    return root
}

```



# [Medium Leetcode 106 Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal)
## 1. 题目要求
中序：左子树 + 根 + 右子树
后序：左子树 + 右子树 + 根

## 2. 解法

```
func buildTree(inorder []int, postorder []int) *TreeNode {
   idxMap := map[int]int{}
    for i, v := range inorder {
        idxMap[v] = i
    }
    var build func(int, int) *TreeNode
    build = func(inorderLeft, inorderRight int) *TreeNode {
        // 无剩余节点
        if inorderLeft > inorderRight {
            return nil
        }

        // 后序遍历的末尾元素即为当前子树的根节点
        val := postorder[len(postorder)-1]
        postorder = postorder[:len(postorder)-1]
        root := &TreeNode{Val: val}

        // 根据 val 在中序遍历的位置，将中序遍历划分成左右两颗子树
        // 由于我们每次都从后序遍历的末尾取元素，所以要先遍历右子树再遍历左子树
        inorderRootIndex := idxMap[val]
        root.Right = build(inorderRootIndex+1, inorderRight)
        root.Left = build(inorderLeft, inorderRootIndex-1)
        return root
    }
    return build(0, len(inorder)-1)
}
```


# [Easy Leetcode 257](https://leetcode-cn.com/problems/binary-tree-paths/)
## 1. 题目要求
给你一个二叉树的根节点 root ，按 任意顺序 ，返回所有从根节点到叶子节点的路径。

叶子节点 是指没有子节点的节点。

## 2. 解法

- 维护一个global param
- path + node.Val
- 若left right == NIL, 到底部了
- 只有左、右为空的，就被吞掉了

```
var paths []string

func binaryTreePaths(root *TreeNode) []string {
    paths = []string{}
    constructPaths(root, "")
    return paths
}

func constructPaths(root *TreeNode, path string) {
    if root != nil {
        pathSB := path
        pathSB += strconv.Itoa(root.Val)
        if root.Left == nil && root.Right == nil {
            paths = append(paths, pathSB)
        } else {
            pathSB += "->"
            constructPaths(root.Left, pathSB)
            constructPaths(root.Right, pathSB)
        }
    }
}
```

# [Medium Leetcode 113](https://leetcode-cn.com/problems/path-sum-ii/)
## 1. 题目要求
给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

## 2. 解法

```
var ret [][]int
func pathSum(root *TreeNode, targetSum int) [][]int {
    ret = [][]int{}
    find(root, []int{}, 0, targetSum)
    return ret
}

func find(root *TreeNode, path []int, sum int, targetSum int) {
    if root == nil {
        return
    }
    sum += root.Val
    path = append(path, root.Val)
    
    if root.Left == nil && root.Right == nil && sum == targetSum {
        ret = append(ret, path)
        return
    }
    find(root.Left, newPath(path), sum, targetSum)
    find(root.Right, newPath(path), sum, targetSum)
}

func newPath(path []int) []int {
    ret := []int{}
    for _, p := range path {
        ret = append(ret, p)
    }
    return ret
}
```


# [Medium Leetcode 437](https://leetcode-cn.com/problems/path-sum-iii/submissions/)
## 1. 题目要求
给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。

路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

## 2. 解法
前缀和
- 记录 root 到当前点（curr_sum) 和
  - curr_sum = n1 + n2 + n3 + ... + n
  - if curr_sum - target_sum == n_K + ... n:
    - 即 从 k -> n 存在一个符合条件的路径

```
func pathSum(root *TreeNode, targetSum int) int {
    ans = 0

    preSum(root, map[int]int{0: 1}, 0, targetSum)
    return ans
}

func preSum(root *TreeNode, preSumMap map[int]int, curr, targetSum int) {
    if root == nil {
        return
    }

    curr = curr + root.Val
    if val, ok := preSumMap[curr-targetSum]; ok {
        ans += val
    }
    preSumMap[curr]++
    preSum(root.Left, preSumMap, curr, targetSum)
    preSum(root.Right, preSumMap, curr, targetSum)
    preSumMap[curr]--   // 遍历完节点，回退到上一个节点，需要删除前缀和
}
```

# [Hard Leetcode 124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
## 1. 题目要求
路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给你一个二叉树的根节点 root ，返回其 最大路径和 。


## 2. 解法
左子树 + 当前点 + 右子树 最大和

```
var maxSum int 

func maxPathSum(root *TreeNode) int {
    maxSum = math.MinInt32

    pathSum(root)

    return maxSum
}

func pathSum(root *TreeNode) int {
    if root == nil {
        return 0
    }

    left := max(pathSum(root.Left), 0)
    right := max(pathSum(root.Right), 0)

    curr := left + right + root.Val
    maxSum = max(curr, maxSum)

    return root.Val + max(left, right)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```


# [Leetcode 98](https://leetcode-cn.com/problems/validate-binary-search-tree/)
## 1. 题目要求
给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。

有效 二叉搜索树定义如下：

节点的左子树只包含 小于 当前节点的数。
节点的右子树只包含 大于 当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。


## 2. 解法

```
func isValidBST(root *TreeNode) bool {
    return valid(root, math.MinInt64, math.MaxInt64)
}

func valid(root *TreeNode, min, max int) bool {
if root == nil {
        return true
    }

    if root.Val >= max || root.Val <= min {
        return false
    }

   return valid(root.Left, min, root.Val) && valid(root.Right, root.Val, max)
}
```




# [Leetcode ]()
## 1. 题目要求


## 2. 解法

```
```

