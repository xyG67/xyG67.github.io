---
layout: post
title: "Leetcode String"
date: 2022-04-10
---

# [Leetcode 674](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/submissions/)
## 1. 题目要求
Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

Example 1:
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
 

Example 2:
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "a
 

即从左到右，查询对称的子数组

## 2. Code
将count作为全局变量，初始值为1。从左到右，逐次检查对称子数组，如果符合要求，就count自增。每次查询时，都从[i, i]和 [i, i+1]处，从内向外寻找，一旦出现不符合要求的情况，就弹出循环。O(N^2)

```
    static int  count = 1;
    public static int countSubstrings(String s) {
        int ans = 0;
        for (int center = 0; center < 2 * s.length() - 1; center++) {
            // 首先是left，有一个很明显的2倍关系的存在，其次是right，可能和left指向同一个（偶数时），也可能往后移动一个（奇数）
            // 大致的关系出来了，可以选择带两个特殊例子进去看看是否满足。
            int left = center / 2;
            int right = left + center % 2;

            while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
                ans++;
                left--;
                right++;
            }
        }
        return ans;
```


# Leetcode 784 排列组合
## 1. 题目要求
Given a string S, we can **transform every letter individually to be lowercase or uppercase** to create another string.  Return a list of all possible strings we could create.
```
Examples:
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

Input: S = "3z4"
Output: ["3z4", "3Z4"]

Input: S = "12345"
Output: ["12345"]
```

Note:

- S will be a string with length between 1 and 12.
- S will consist only of letters or digits.

## 2. 解法
### 遍历
- 使用Queue保存结果。
- 遍历字符串，确保扫描到每个字符。
- 当前字符为字母，将Queue尾部的结果弹出。
- 更改弹出字符串 i 处的字符，分别插入小写和大写版本。

 ```
 public static List<String> letterCasePermutation(String S) {
       
        if(S == null) return new ArrayList<>(Arrays.asList(S));
        
        Queue<String> queue = new LinkedList<>();
        queue.offer(S);   // 将字符串插入队列
        for(int i = 0 ; i < S.length() ; i++) {
            if(S.charAt(i) >= '0' && S.charAt(i) <= '9') continue;
            int size = queue.size();
            for(int j = 0 ; j < size ; j++) {
                String curr  = queue.poll();
                char[] chs = curr.toCharArray();
                
                chs[i] = Character.toLowerCase(chs[i]); // 小写版本
                queue.offer(String.valueOf(chs));
                
                chs[i] = Character.toUpperCase(chs[i]); // 大写版本
                queue.offer(String.valueOf(chs));
            }
        }
        
        return new LinkedList<>(queue);
    }
 ```

详解
```
`1abc` 

Init -> queue = ['1abc'] 

loop1 -> queue = ['1abc']

loop2 -> queue = [] -> queue = ['1abc', '1Abc']

loop3 -> queue = ['1abc'] -> queue = ['1Abc', '1abc', '1aBc'] -> queue = ['1abc', '1aBc'] -> queue = ['1abc', '1aBc', '1Abc', '1ABc'] 

...
```

 是很粗暴的遍历, 两次遍历接近于 O（n!）？

