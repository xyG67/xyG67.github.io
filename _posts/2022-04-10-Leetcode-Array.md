---
layout: post
title: "Leetcode Array"
date: 2022-04-10
---
https://darktiantian.github.io/LeetCode%E7%AE%97%E6%B3%95%E9%A2%98%E6%95%B4%E7%90%86%EF%BC%88%E6%95%B0%E7%BB%84%E7%AF%87%EF%BC%89Array/


# 滑动窗口
- 长度最小的子数组（209 Medium）
  - n个正整数数组，找出满足和>S的最短连续子数组，返回长度


# [Leetcode 26](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)
## 1. 题目要求
给你一个 升序排列 的数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 

## 2. 解法
快慢指针


```
func removeDuplicates(nums []int) int {
    cnt := len(nums)
    if cnt <= 1 {
        return cnt
    }

    slow := 0
    fast := 1

    pre := nums[slow]
    for fast < cnt {
        for fast < cnt && nums[fast] == pre {
            fast++
        } 

        if fast > cnt - 1 {  // 注意一下，break只能弹出一层
            break
        }
        slow++
        nums[slow] = nums[fast]
        pre = nums[slow]

        fast++
    }
    return slow + 1
}
```

# [Leetcode 54](https://leetcode-cn.com/problems/spiral-matrix/)

## 1. 题目要求
给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。

## 2. 解法

TIPS: 
- 维护direction列表，轻易切换方向
- 记录访问过的点，控制转向


```
func spiralOrder(matrix [][]int) []int {
   if len(matrix) == 0 || len(matrix[0]) == 0 {
		return []int{}
	}

	var (
		ret          = []int{}
		direction    = [][]int{[]int{0, 1}, []int{1, 0}, []int{0, -1}, []int{-1, 0}}
		directionIdx = 0

		visited = [][]int{}

		rowLen = len(matrix)
		colLen = len(matrix[0])
		total  = rowLen * colLen

		row, col = 0, 0
	)

	for i := 0; i < len(matrix); i++ {
		visited = append(visited, make([]int, len(matrix[0])))
	}

	for i := 0; i < total; i++ {
		ret = append(ret, matrix[row][col])
		visited[row][col] = 1
		nextRow := direction[directionIdx][0] + row
		nextCol := direction[directionIdx][1] + col

		if nextCol < 0 || nextCol > len(matrix[0])-1 ||
			nextRow < 0 || nextRow > len(matrix) - 1||
			visited[nextRow][nextCol] == 1 {
			directionIdx = (directionIdx + 1) % 4
		}
		row = direction[directionIdx][0] + row
		col = direction[directionIdx][1] + col
	}

	return ret
    
}
```

# [Leetcode 59](https://leetcode-cn.com/problems/spiral-matrix-ii/)
## 1. 题目要求
给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。


## 2. 解法

TIPS: 
- 同54


```
func generateMatrix(n int) [][]int {
	ret := make([][]int, 0)  // 注意这里是0
	visited := make([][]int, 0)
	for i := 0; i < n; i++ {
		ret = append(ret, make([]int, n))
		visited = append(visited, make([]int, n))
	}

	var (
		direction    = [][]int{[]int{0, 1}, []int{1, 0}, []int{0, -1}, []int{-1, 0}}
		directionIdx = 0

		total = n * n

		row, col = 0, 0
	)

	for i := 1; i <= total; i++ {
		ret[row][col] = i
		visited[row][col] = 1

		nextRow := direction[directionIdx][0] + row
		nextCol := direction[directionIdx][1] + col

		if nextCol < 0 || nextCol > n-1 ||
			nextRow < 0 || nextRow > n-1 ||
			visited[nextRow][nextCol] == 1 {
			directionIdx = (directionIdx + 1) % 4
		}
		row = direction[directionIdx][0] + row
		col = direction[directionIdx][1] + col
	}
	return ret
}
```

# [Leetcode 80](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)
## 1. 题目要求
给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

## 2. 解法

比较简单的方法是 和I-2位的比较大小

```
func removeDuplicates(arr []int) int {
    if len(arr) < 3 {
		return len(arr)
	}

	i := 0

	for _, num := range arr {
		if i < 2 || num > arr[i-2] {
			arr[i] = num
			i++
		}
	}
    return i
}
```



# [Leetcode 66](https://leetcode-cn.com/problems/plus-one/)
## 1. 题目要求
给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。

## 2. 解法
简单看一下就好了。。

```
func plusOne(digits []int) []int {
    ret := arr

	arr[len(arr)-1] += 1

	plus := 0
	for i := len(arr) - 1; i >= 0; i-- {
		ret[i] = ret[i] + plus

		plus = ret[i] / 10
		ret[i] = ret[i] % 10
	}
	if plus > 0 {
		ret = append([]int{1}, ret...)
	}
	return ret
}
```


# [easy Leetcode 169](https://leetcode-cn.com/problems/majority-element/)
## 1. 题目要求
给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。


## 2. 解法

```
func majorityElement(nums []int) int {
    cnt := 0
	candidate := 0

	for _, num := range nums {
		if cnt == 0 {
			candidate = num
		}
		if num == candidate {
			cnt++
		} else {
			cnt--
		}
	}
	return candidate
}
```


# [Leetcode 989](https://leetcode-cn.com/problems/add-to-array-form-of-integer/)
## 1. 题目要求
整数的 数组形式  num 是按照从左到右的顺序表示其数字的数组。

例如，对于 num = 1321 ，数组形式是 [1,3,2,1] 。
给定 num ，整数的 数组形式 ，和整数 k ，返回 整数 num + k 的 数组形式 。

示例 1：

输入：num = [1,2,0,0], k = 34
输出：[1,2,3,4]
解释：1200 + 34 = 1234

## 2. 解法
66的衍生题

```
func addToArrayForm(nums []int, k int) []int {
    if k == 0 {
        return nums
    }
    i := len(nums) - 1
	for k > 0 {
        if i >= 0 && i < len(nums) {
		    nums[i] = nums[i] + k%10
        } else {
            nums = append([]int{k%10}, nums...)
        }
		k = k / 10
		i--
	}

	ret := nums
	plus := 0
	for i := len(nums) - 1; i >= 0; i-- {
		ret[i] = ret[i] + plus

		plus = ret[i] / 10
		ret[i] = ret[i] % 10
	}
	if plus > 0 {
		ret = append([]int{1}, ret...)
	}
	return ret
}
```

# [Leetcode 88](https://leetcode-cn.com/problems/merge-sorted-array/)
## 1. 题目要求
合并两个有序数组，在nums1上修改。原题
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]

## 2. Code
def merge(self, nums1, m, nums2, n):
    while m>0 and n>0:
        if nums1[m-1] > nums2[n-1]:
            nums1[n+m-1] = nums1[m-1]
            m -= 1
        else:
            nums1[n+m-1] = nums2[n-1]
            n -= 1
    if n > 0:
        nums1[:n] = nums2[:n]

**衍生：合并K个有序数组**

# Binary Search


# [Leetcode 496](https://leetcode-cn.com/problems/next-greater-element-i/)
## 1. 题目要求
You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

有A,B两个数组，A中的元素在B中都存在，且两个数组内元素没有重复。现在要找到A中所有元素，在B中对应的位置上，是否右侧第一个比它大的数字。
 
示例 1：

```
输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
```


示例 2：

```
输入：nums1 = [2,4], nums2 = [1,2,3,4].
输出：[3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 2 ，用加粗斜体标识，nums2 = [1,2,3,4]。下一个更大元素是 3 。
- 4 ，用加粗斜体标识，nums2 = [1,2,3,4]。不存在下一个更大元素，所以答案是 -1 。
```

## 2. Code
HashMap存 element:index, 先遍历B，再遍历A。

Java


```
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer, Integer> num_r = new HashMap<Integer, Integer>();
        Stack<Integer> stack = new Stack<>();
        
        for(int num : nums2) {
            while(!stack.isEmpty() && stack.peek() < num) {
                num_r.put(stack.pop(), num);
            }
            stack.push(num);
        }
        
        for(int i = 0 ; i < nums1.length ; i++) {
            nums1[i] = num_r.getOrDefault(nums1[i], -1);
        }
        return nums1;
    }
```

Golang
- 没有原生STACK API。。
- 遍历B：
  - 维护一个MAP、一个ARR。从后向前遍历数组。
  - 若B[i] <= B[len（B） - 1]（即 i 右侧递增），则正常压栈。
  - 若B[i] > B[i - 1] (即 i 右侧递减)，则从arr右侧POP元素，直到arr尾部数字最大


```
func nextGreaterElement(nums1 []int, nums2 []int) []int {
    mp := map[int]int{}
    stack := []int{}
    for i := len(nums2) - 1; i >= 0; i-- {
        num := nums2[i]
        for len(stack) > 0 && num >= stack[len(stack)-1] {
            stack = stack[:len(stack)-1]
        }
        if len(stack) > 0 {
            mp[num] = stack[len(stack)-1] // 取当前栈尾部的元素
        } else {
            mp[num] = -1
        }
        stack = append(stack, num)
    }

    res := make([]int, len(nums1))
    for i, num := range nums1 {
        res[i] = mp[num]
    }
    return res
}
```

# [Medium Leetcode 220](https://leetcode-cn.com/problems/contains-duplicate-iii/)
## 1. 题目要求
给你一个整数数组 nums 和两个整数 k 和 t 。请你判断是否存在 两个不同下标 i 和 j，使得 abs(nums[i] - nums[j]) <= t ，同时又满足 abs(i - j) <= k 。

如果存在则返回 true，不存在返回 false。

##
- Bucket Sort，把元素放入 代表不同number的bucket。
- 本题中，把数字/t + 1作为桶的index，数字作为val，放入桶中
- 在遍历数组的过程中，检查当前数字/（t+1）是否在下角标内存在，如果存在，即存在满足条件的值（差<t）
- 遍历过程中，删除距离过远（nums[i-k]/(t+1)）的桶里的数字（因不满足 相距k以内的条件）
- 数字不存在，将数丢到对应桶里
- 并且，检查相邻的桶里是否有符合要求的数字

```
func containsNearbyAlmostDuplicate(nums []int, k int, t int) bool {
    bucket := make(map[int]int, 0) // key: val, val: idx

	width := t + 1 // 桶的容量（？
	for i, num := range nums {
		buck := getID(i, width)

		if _, ok := bucket[buck]; ok {
			return true
		} 

		if val, ok := bucket[buck-1]; ok && num-val <= t {
			return true
		}
		if val, ok := bucket[buck+1]; ok && val-num <= t {
			return true
		}
        bucket[buck] = num

        if i >= k {
			delete(bucket, getID(nums[i-k], t+1)) // 窗口滑出范围
		}
	}
	return false
}

func getID(x, w int) int {
    if x >= 0 {
        return x / w
    }
    return (x+1)/w - 1
}
```

# [Leetcode 575](https://leetcode-cn.com/problems/distribute-candies/)
## 1. 题目要求
Alice 有 n 枚糖，其中第 i 枚糖的类型为 candyType[i] 。Alice 注意到她的体重正在增长，所以前去拜访了一位医生。

医生建议 Alice 要少摄入糖分，只吃掉她所有糖的 n / 2 即可（n 是一个偶数）。Alice 非常喜欢这些糖，她想要在遵循医生建议的情况下，尽可能吃到最多不同种类的糖。

给你一个长度为 n 的整数数组 candyType ，返回： Alice 在仅吃掉 n / 2 枚糖的情况下，可以吃到糖的 最多 种类数。

示例 1：

输入：candyType = [1,1,2,2,3,3]
输出：3
解释：Alice 只能吃 6 / 2 = 3 枚糖，由于只有 3 种糖，她可以每种吃一枚。
示例 2：

输入：candyType = [1,1,2,3]
输出：2
解释：Alice 只能吃 4 / 2 = 2 枚糖，不管她选择吃的种类是 [1,2]、[1,3] 还是 [2,3]，她只能吃到两种不同类的糖。
示例 3：

输入：candyType = [6,6,6,6]
输出：1
解释：Alice 只能吃 4 / 2 = 2 枚糖，尽管她能吃 2 枚，但只能吃到 1 种糖。

## 2. 解法

```
public int distributeCandies(int[] candies) {
        HashSet<Integer> s = new HashSet<>();
        for(int c : candies) {
            s.add(c);
        }
        return candies.length/2 > s.size()? s.size():candies.length/2;
    }
```


https://juejin.cn/post/7031351811898343460


# [Leetcode 448 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/solution/)
## 1. 题目要求
给你一个含 n 个整数的数组 nums ，其中 nums[i] 在区间 [1, n] 内。请你找出所有在 [1, n] 范围内但没有出现在 nums 中的数字，并以数组的形式返回结果。


示例 1：

输入：nums = [4,3,2,7,8,2,3,1]
输出：[5,6]
示例 2：

输入：nums = [1,1]
输出：[2]

要求：不使用额外的空间

## 2. 解法    
鸽笼原理
由题意可得，数组有1~n个笼子，如果出现过，相应的“鸽笼”就会被占掉，即将对应位置数字加N。 
最后遍历一遍，如果“鸽笼”数字小于N，即没出现的该数字。

```
func findDisappearedNumbers(nums []int) []int {
    n := len(nums)
    for _, num := range nums {
        val := (num - 1) % n
        nums[val] += n
    }

    fmt.Print(nums)
    ret := []int{}
    for i, num := range nums {
        if num <= n {
            ret = append(ret, i + 1)
        }
    }
    return ret
}
```

# [Leetcode 179 最大数](https://leetcode-cn.com/problems/largest-number/)
## 1. 题目要求
给定一组非负整数 nums，重新排列每个数的顺序（每个数不可拆分）使之组成一个最大的整数。

注意：输出结果可能非常大，所以你需要返回一个字符串而不是整数。

 

示例 1：

输入：nums = [10,2]
输出："210"
示例 2：

输入：nums = [3,30,34,5,9]
输出："9534330"


## 2. 解法    

merge sort 修改比较器

```

func maxLeftNum(a, b int) int {
    sa, sb := 10, 10
    for sa <= a {
        sa = sa * 10
    }
    for sb <= b {
        sb = sb * 10
    }

    if sa * b + a > sb * a + b {
        return b
    } else {
        return a
    }
}

```


# [Medium Leetcode 347 top k freqenct](https://leetcode-cn.com/problems/top-k-frequent-elements/)
## 1. 题目要求

给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

 

示例 1:

输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
示例 2:

输入: nums = [1], k = 1
输出: [1]


## 2. 解法    

Priority Queue
需要使用heap库实现PQ（修改比较器 和 push pop函数）。

先计数，再push到pq内，最后输val前k个的key。

类似的题包括 692 https://leetcode-cn.com/problems/top-k-frequent-words/ 

```
type Item struct {
	Key   int
	Count int
}

// A PriorityQueue implements heap.Interface and holds Items.
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int {
	return len(pq)
}

func (pq PriorityQueue) Less(i, j int) bool {
	fmt.Println("???")
	return pq[i].Count > pq[j].Count
}

func (pq PriorityQueue) Swap(i, j int) {
	pq[i], pq[j] = pq[j], pq[i]
}

// Push define
func (pq *PriorityQueue) Push(x interface{}) {
	item := x.(*Item)
	*pq = append(*pq, item)
}

// Pop define
func (pq *PriorityQueue) Pop() interface{} {
	n := len(*pq)
	item := (*pq)[n-1]
	*pq = (*pq)[:n-1]
	return item
}

```