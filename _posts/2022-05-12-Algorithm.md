---
layout: post
title: "Algorithm"
date: 2022-05-12
---


很不正式的总结一下算法题


1. Tree

   1) Traverse
    从上到下压栈，从下向上弹出。和 DFS 思路差不多
    多可用于处理需要遍历树的问题
        - 递归遍历点（前序、中序、后序）
        - 求根节点-子节点Sum
        - 指定路径
        - 寻找目标节点

    部分题需要通过全局变量作为辅助，比如保存遍历过、符合要求的路径（golang可以通过在函数内定义func来避免全局变量，虽然这两种方法都不美观）

    算法：

        - 一棵二叉树根为 root 。删除 1 条边，使二叉树分裂成两棵子树，且它们子树和的乘积尽可能大。
          - 先求总和
          - 遍历求差值，途中顺势通过比较大小，求子树和最大乘积
            - curr_node_sum = total_sum - curr_left_sum - curr_right_sum - curr_val
        - 求子树和
        - 求总和
        - 遍历求路径和
        - **使用循环替代递归**
        - 镜像树：遵循BFS的思路，颠倒压栈。

   2) 前缀和

    统计二叉树种值为Target的合。
        - 用Map存储【当前Node值】，key-求和， val-个数
          - 每到一个点，向下递归，继续往MAP里存和，自增 val
          - 向上返回时，递减val
        - 每到一个点，只需要查询 map[target-curr] 是否有值即可


    3）BFS

    用Queue辅助实现，可用于求层级
    缺点是不能用于查询 root-node 节点，从上到下的连通关系的题。

    4）二叉搜索树

    右边永远大于左边。
    中序遍历：从小到大，（左中右）

    删除二叉搜索树中的某个节点：先找到对应点，找到后，用该点右节点作为新点。右子树最左边的节点（当前右边最小，左边最大）作为新node的左子树。
    二叉搜索树第k大 ：二叉搜索树，右中左traverse是从大到小。



2. Array

    1）Binary Search

    找中点，减小边界

    变种题：
        递增序列，找是否存在某个值
        递增序列，中间切断，拼接到数组头部，找某个值

    需要确认：是否有重复数值

    2）滑动窗口

        1) 可变长度的滑动窗口
           1) 有序数组，删除重复出现的元素 
              1) 延申题：删除重复出现的元素，使每个元素最多出现两次（窗口大小2）


           2) 区间子数组个数 https://leetcode.cn/problems/number-of-subarrays-with-bounded-maximum/
              1) 整数数组 nums 和两个整数：left 及 right 。找出 nums 中连续、非空且其中最大元素在范围 [left, right] 内的子数组，并返回满足条件的子数组的个数。
              2）从左向右遍历，维护滑动窗口，针对符合条件的子数组：count += right-left+1 （类似：2 5 6有 2/2 5/ 2 5 6 三种选择）
           3) 978. 最长湍流子数组 https://leetcode.cn/problems/longest-turbulent-subarray/

                ```
                    if (arr[right - 1] < arr[right] && arr[right] > arr[right + 1]) {
                        right++;
                    } else if (arr[right - 1] > arr[right] && arr[right] < arr[right + 1]) {
                        right++;
                    } else {
                        left = right;
                    }
                ```

            4) 无重复字符的最长字串 https://leetcode.cn/problems/longest-substring-without-repeating-characters/
                给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。
                - loop
                - 通过MAP记录字符位置: key: character, val: current index
                - if current index exist: left = max(idx+1, left) // 注意一下左边窗口的滑动方式
 

        2) 固定长度的滑动窗口 

           1) K长度滑动窗口元素最大值 https://leetcode.cn/problems/sliding-window-maximum/
              1) 滑动窗口，辅助使用**递减数组**(单调栈) 保留窗口内最大值。


    【TBD】寻找符合条件的最短字符串：可伸缩的窗口遍历字符串，时间复杂度大致为O(n)。适用于“寻找符合某条件的最小子字符串”题型。
        - 条件
          - 包含固定字母
          - 子数组和等于给定值：https://leetcode.cn/problems/minimum-size-subarray-sum/
          - 子字符串同时包含大写、小写(?


    3）Bucket

    把元素放入 代表不同number的bucket。即取即用，一种类似hashmap的实现

    题目：

        - 求符合要求的字符串子序列的个数（不要求连续序列，可以删掉一些字符，但不能改变相对顺序） https://leetcode.cn/problems/number-of-matching-subsequences/

          - s = "abcde", words = ["a","bb","acd","ace"] output=3。
          - 把words字符丢到长为26的桶。
          - 遍历s的每个字符，到桶内找子字符。把子字符的首个字符去掉，剩下的字符丢到剩下的桶里（例如：acd ==匹配a==> cd）
          - 桶内子字符都匹配过，符合条件

    4）Priority Queue

    统计频次：PriorityQueue top k freqenct

    1) 排序

    Merge Sort

        - sort左边、右边，merge两边在一起
        - 衍生题：
          - LC88：合并两个有序数组，[1,2,3,0,0,0] + [2,5,6] => [1,2,2,3,5,6]。需要从后往前合并。 O（1）空间复杂度时（类似滑动窗口)

          - LC179：返回数组里每个数能组成的最大数。修改sort比较器     

    6）DP 动态规划
    维护数字或一个数组，存储前一个数的结果。可用于统计最大和、最大乘积

      - 题目：

        1） 最大连续子数组和：https://leetcode.cn/problems/maximum-subarray/
          - 贪心算法：
            - 当前值SUM < 0, 丢弃前值
            - curr_sum := nums[0]
            - curr_sum = max(nums[i], curr_sum + nums[i])
            - max_sum = max(curr_sum, max_sum)
          - DP:
            - dp[i+1] = max(dp[i]+nums[i], nums[i]);

            最小连续子数组和：
            - 贪心：
              - min = min(nums[i], min+nums[i])

            环形数组的最大连续子数组和：LC918
                - 计算无环最大连续子数组和
                - 计算有环最大连续子数组和 = 总和 - 最小连续子数组和

       2） 乘积最大子数组: https://leetcode.cn/problems/maximum-product-subarray/

          - 类似最大子数组和：
            - 维护一个最大值 和 一个最小值。当出现nums[i]<0时，交换max和min值
            - max = max(nums[i], max * nums[i])
            - min = min(nums[i], min * nums[i])
            - ret = max(max, ret)

       3） 打家劫舍I/II/III：Input：无序数组。求不相邻Index的最大和（进阶：第一个和最后一个是相邻的）

          - https://leetcode.cn/problems/house-robber/submissions/
          - https://leetcode.cn/problems/house-robber-ii/
          - https://leetcode.cn/problems/house-robber-iii/ Input为树结构，求所有不相连的点的最大和
          - 解题思路：
            - 构建DP：长度为数组长度+1（初始值0，INDEX1=nums[0]）
            - input:[1,2,3,4] => dp[0, 1, 2, 4, 6] =>  6
            - dp[i+1] = max(dp[i], dp[i-1]+nums[i]) (要或者不要当前一格)
            - 延申1：前后能不相邻，需要计算两种情况，1）PICK第二个到最后[1:] 2）PICK第一个到倒数第二个 [:n-1]。构造两个DP
            - 延申2：树结构，
              - 1) 递归：sum := rob(root.left.left) + rob(root.left.right) + rob(root.right.left) + rob(root.right.right). 
                    - 对比当前点+4个孙子节点 和 2个孩子节点 Sum
              - 2）DP：[]int{} // sum with unselected, selected current node

                ```
                func rob(root *TreeNode) int {
                    //sumMap = map[*TreeNode]int{}
                    ret := optRob(root)
                    return max(ret[0], ret[1])
                }

                func optRob(root *TreeNode) []int {
                    if root == nil {
                        return []int{0, 0} // unselected, selected
                    }

                    right := optRob(root.Right)
                    left := optRob(root.Left)

                    selected := root.Val + right[0] + left[0]
                    unselected := max(right[0], right[1]) + max(left[0], left[1])

                    return []int{unselected, selected}
                }
                ```
            
            1) 给定数组，求三个不重叠的子数组最大和 https://leetcode.cn/problems/maximum-sum-of-3-non-overlapping-subarrays/solution/gong-shui-san-xie-jie-he-qian-zhui-he-de-ancx/
               1) 转移方程：
                  1) dp[i][j]​ 表示到数组第 j​ 个元素为止，前 i​ 个互不重叠的子数组的最大值（i =[0,2]）
                  2) dp[i][j]=max(dp[i][j−1],dp[i−1][j−k]+sum[j]−sum[j−k])
                    
     1) 回溯
   
        暴力求解排列组合的N种结果集（解决非连续子数组，即全部元素打散，连续的用for loop + 滑动窗口解决）
            - 题目：
                1） 求数组子集 https://leetcode.cn/problems/subsets/ 
                    - 回溯，子方法传入array，向下找时append，找完remove掉。（实际也可以用两个FOR LOOP暴力解）

                        ```
                            var res [][]int

                            func subsets(nums []int) [][]int {
                                res = make([][]int, 0)

                                dfs(nums, 0, []int{})
                                return res
                            }

                            func dfs(nums []int, start int, list []int) {
                                res = append(res, copy(list))

                                for i := start; i<len(nums); i++ {
                                    list = append(list, nums[i])
                                    dfs(nums, i+1, list)
                                    list = list[:len(list)-1]
                                }    
                            }

                            func copy(old []int) []int {
                                ret := make([]int, 0, len(old))
                                for _, num := range old {
                                    ret = append(ret, num)
                                }
                                return ret
                            }
                        ```
                        相关题：乘积小于K的子数组：https://leetcode.cn/problems/subarray-product-less-than-k/

    7）前缀和

    暴力排列组合耗时长，求和为K的问题可以考虑使用前缀和
       - 题目
            1） 和为 K 的子数组：给你一个整数数组 nums 和一个整数 k ，统计并返回 该数组中和为 k 的子数组的个数。 https://leetcode.cn/problems/subarray-sum-equals-k/

                ```
                func subarraySum(nums []int, k int) int {
                    count, pre := 0, 0
                    m := map[int]int{}  // key: sum, val: count
                    m[0] = 1
                    for i := 0; i < len(nums); i++ {
                        pre += nums[i]
                        if _, ok := m[pre-k]; ok {  // 当前总和-K=差值，查询是否存在过该差值（2Sum的思路）
                            count += m[pre-k]       // match!
                        }
                        m[pre] += 1                 // 前缀和出现次数（有负数）
                    }
                    return count
                }
                ```

                - 衍生题：974 元素和可被K整除的子数组。Map：Key：sum % K, Val: count. 
                  - 相同余数的子数组个数大于等于2时，任意选取其中两个子数组余数相减，即余数抵消，可得到一个符合题目要求的sum。（此处的个数计算方式为：n*(n-1) / 2）

   1) 单调栈

    单调递增栈：栈底>栈顶，peek > new
    单调递减栈：栈底<栈顶, peek < new
    
    递减栈：
        求index左边的小数 Previous Less Element

        ```
        vector<int> previous_less(A.size(), -1);
        for(int i = 0; i < A.size(); i++){
            while(!in_stk.empty() && A[in_stk.top()] > A[i]){
                in_stk.pop();  //  栈顶偏大的数被丢掉，获得i前面比i小的数
            }
            previous_less[i] = in_stk.empty()? -1: in_stk.top();  // 栈顶的数偏小
            in_stk.push(i);    // 压栈
        }
        ```

        求index右边的小数 Next Less Element

        ```
        vector<int> previous_less(A.size(), -1);
        for(int i = 0; i < A.size(); i++){
            while(!in_stk.empty() && A[in_stk.top()] > A[i]) {
                auto x = in_stk.pop();    // 弹栈，栈里index对应的更小的数 == 当前数                 
                next_less[x] = i;
            }
            in_stk.push(i);  // 压栈
        }
        ```

    - 题目
      - LC 496,两个无序数组，A中所有元素在B中对应位置（VAL相等），右边第一个比该数字大的数
            - 从后向前遍历，构造递增栈。
            - 用MAP存num和右侧第一个大的数，key: num, val: stack.peek。如果栈为空，存DEFAULT值-1。
            - STACK压栈num
            - 再遍历A，从MAP取答案

      - LC 907 子数组最小值的和 https://leetcode.cn/problems/sum-of-subarray-minimums/
        - 单独求Previous Less Element 和 Next Less Element
        - 第i个数字的和是 (A[i]*next_less[i]*previous_less[i])，一次遍历求得答案

    9）其他

    多用Queue、Stack、Map、Set、临时变量处理的题

        - plus one
        - LC 448 找到数组中所有消失的数字。区间[1,N]。 
          - 鸽笼原理。数组有1~n个笼子，如果出现过，相应的“鸽笼”就会被占掉，即将对应位置数字加N。 最后遍历一遍，如果“鸽笼”数字小于N，即没出现的该数字。

       - 无序数组，求数字连续递增的最大数字子集 O（n）https://leetcode.cn/problems/longest-consecutive-sequence/
         - Set，while loop 持续寻找加减1的数字。

       - 逆波兰表达式求值：tokens = ["2","1","+","3","*"] ((2 + 1) * 3) = 9  https://leetcode.cn/problems/evaluate-reverse-polish-notation/

       - 寻找两个有序数组中位数：
           1）Merge Sort，找中间的点
           2）进阶：lg(m+n)https://zhuanlan.zhihu.com/p/79687649


3. List

    1）递归&&迭代

    删除重复结点，从前向后执行。先根据当前参数(NODE, NODE.NEXT是否相等)，再向后执行。node.next = deletenode(xxxx)

    复制带随机点的链表。用MAP存点（key:node, val: newNode）。和上面题一样，用指针指向函数返回值。newHead.Next = copy(head.Next); newHead.Random = copy(head.Random)

    2）反转链表

    确定三个点：pre、curr_head、next，把几个点位置换一下
    pre -> node_n -> head -> next

    pre -> next -> node_n -> head

    1) 双指针&&快慢指针
   
    分隔列表（一个指针存小数，一个存大数
    求是否闭环、求闭环交点
    找中间点、衍生：判断是否回文：找到中间点、reverse、对比是否相同

    4） merge sort

    找到中间点、sort两边、merge
    - 衍生题：
       1) 求出数组中的逆序对的总数。逆序对：数组中的两个数字，前面数字大于后面的数字。https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/
        - merge子数组时：若右边元素 > 小于左边元素。则右边元素和左边所有元素都能组成逆序对。count += mid-leftIdx+1



4. Graph

   1) BFS&&DFS

    DFS用递归压栈，压栈前，记得把visited置为true。

   2）图论

   - DP：matrics 迷宫求最小路径和：dp[i][j] = min(next_step_1, next_step_2) + cost_in_step[i][j]
   - Dijkstra: 本质依然是上面的思路，但用一维数组存点和点的联通成本 n^2，用PQ维护cost时，能优化到nlgn
   - 拓扑排序：O（V+E）。BFS:Queue存结果，用[]int记录每个点的入度，再根据点依次递减入度。到0时可以放入结果。
  

5. String


6. 其他
- LRU（Prefer MAP+Linkedlist）


待处理：
    
    
     两数组交集 
      平衡二叉树

      https://zhuanlan.zhihu.com/p/402348214

      https://www.cnblogs.com/liuzhen1995/p/11789339.html

