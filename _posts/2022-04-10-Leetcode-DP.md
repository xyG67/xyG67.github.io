---
layout: post
title: "Leetcode NP"
date: 2022-04-10
---

# Leetcode 788

## 1. 题目要求
求数字翻转后的有效数字个数

 X is a good number if after rotating each digit **individually** by 180 degrees, we get a **valid number** that is **different from X**. 

Each digit must be rotated - we cannot choose to leave it alone.

A number is valid if each digit remains a digit after rotation. 0, 1, and 8 rotate to themselves; 2 and 5 rotate to each other; 6 and 9 rotate to each other, and the rest of the numbers do not rotate to any other number and become invalid.

**Question**: Now given a positive number N, how many numbers X from 1 to N are good?

N 取值范围 [1, 10000]


## 2. 解法

最粗暴的解法是从1-10000，逐个判断数字是否为“好数”，时间复杂度为 o(N * LogN)。

优化解法是使用**动态规划**逐步分解问题。根据题目条件可得知，翻转后：

- 2, 5, 6, 9 是好数
- 0, 1, 8 不变，但是为有效数字
- 2，5，7 是无效数字

初始化一个int array，长度为N+1，从0开始递增N。

- 个位 0-9 标记 
  - 0，1，8 为1
  - 2, 5, 6, 9 为2
  - 其他为0

- 当数字大于9时，则判断个位数与其他位数是否符合要求，例如
  - DP方程：
    - int a = dp[i/10]
    - int b = dp[i%10]
    - if a == 1 && b == 1 则为有效数
    - if a >= 1 && b >= 1 则为好数

  - 前10个数字
    - [1, 1, 2, 0, 0, 2, 2, 0, 1 , 2]
  
  - 后续：
    - 10 -> a = dp[1], b = dp[0] -> dp[10] = 有效数
    - 101 -> a = dp[10], b = dp[1] -> dp[101] = 有效数

    - 每个数只需要分别验证2次，复杂度O（lgN）

```
public int rotatedDigits(int N) {
        int res = 0;
        int[] dp = new int[N + 1];
        
        for(int i = 0 ; i <= N ; i++) {
            if( i < 10 ) {
                if(i == 0 || i == 1 || i == 8) dp[i] = 1;
                if(i == 2 || i == 6 || i == 5 || i == 9) { dp[i] = 2; res++;}
            } else {
                int a = dp[i/10];
                int b = dp[i%10];
                
                if(a == 1 && b == 1) dp[i] = 1;
                else if(a >= 1 && b >= 1) {
                    dp[i] = 2;
                    res ++;
                }
            }
        }  
        return res;
    }
```