---
layout: post
title: "Leetcode List"
date: 2022-04-19
---

# [Leetcode 21](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
## 1. 题目要求
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

1->2->4
1->3->4

1->1->2->3->4->4

## 2. 解法

```
func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    res := &ListNode{}

    head := res
    for {
        if list1 == nil && list2 == nil {
            break
        } else if list1 == nil  {
            res.Next = list2
            break
        } else if list2 == nil {
            res.Next = list1
            break
        }

        currVal := 0
        if list1.Val > list2.Val {
            currVal = list2.Val
            list2 = list2.Next
        } else {
            currVal = list1.Val
            list1 = list1.Next
        }

        res.Next = &ListNode{Val: currVal}
        res = res.Next
    }

    return head.Next
}
```


# [Leetcode 82 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
## 1. 题目要求
给定一个已排序的链表的头 head ， 删除**原始链表**中所有重复数字的节点，只留下不同的数字 。返回 已排序的链表 。

1->2->3->3->4->4->5
1->2->5

## 2. 解法
删除重复元素。
- head -> next -> next.Next
- 返回head
- 向下传递next
  - 遇到head == next, 丢掉向后重复元素
    - head = next
  - head != next, head.Next = next(原序)

```
func deleteDuplicates(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }

    next := head.Next
    if head.Val == next.Val {
        for next != nil && head.Val == next.Val {
            next = next.Next
        }
        head = deleteDuplicates(next)
    } else {
        head.Next = deleteDuplicates(next)
    }
    
    return head
}
```


# [Leetcode 83](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
## 1. 题目要求
给定一个已排序的链表的头 head ， 删除所有重复的元素，使每个元素只出现一次 。返回 已排序的链表 。

1->1->2
1->2

## 2. 解法
同82
```
func deleteDuplicates(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }

    deleteNode(head, head.Next)
    return head
}

func deleteNode(node *ListNode, next *ListNode) *ListNode {
    if next == nil {
        node.Next = nil
        return nil
    }
    if node.Val != next.Val {
        node.Next = next
        return deleteNode(next, next.Next)
    }
    return deleteNode(node, next.Next)
}
```


# [Leetcode 86 分隔链表](https://leetcode-cn.com/problems/partition-list/)
## 1. 题目要求
给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你应当 保留 两个分区中每个节点的初始相对位置。

输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]

## 2. 解法
双指针，一个存小数，一个存大数

最后把两个链表连接起来。

```
func partition(head *ListNode, x int) *ListNode {
    if head == nil {
        return head
    }
    small, large := &ListNode{}, &ListNode{}

    smallHead := small
    largeHead := large

    for head != nil {
        if head.Val < x {
            small.Next = head
            small = small.Next
        } else {
            large.Next = head
            large = large.Next
        }

        head = head.Next
    }

    small.Next = largeHead.Next
    large.Next = nil
    
    return smallHead.Next
}
```

# [Leetcode 92](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
## 1. 题目要求
给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表 。


## 2. 解法
置换顺序

```
func reverseBetween(head *ListNode, left int, right int) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }

    dummy := &ListNode{}
    dummy.Next = head

    pre := dummy
    for i := 1; i < left; i++ {
        pre = pre.Next
    }

    head = pre.Next
    for i := left; i < right; i++ {
        tmp := head.Next
        head.Next = tmp.Next
        tmp.Next = pre.Next
        pre.Next = tmp
    }

    return dummy.Next
}
```

# [Leetcode 138](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
## 1. 题目要求
给你一个长度为 n 的链表，每个节点包含一个额外增加的随机指针 random ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 深拷贝。 深拷贝应该正好由 n 个 全新 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 next 指针和 random 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。复制链表中的指针都不应指向原链表中的节点 。

例如，如果原链表中有 X 和 Y 两个节点，其中 X.random --> Y 。那么在复制链表中对应的两个节点 x 和 y ，同样有 x.random --> y 。

返回复制链表的头节点。

用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：

val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。
你的代码 只 接受原链表的头节点 head 作为传入参数。

![image](https://i.postimg.cc/Wz3Hkwc6/lc138.png)

## 2. 解法

```
var cachedNode map[*Node]*Node

func copyRandomList(head *Node) *Node {
	cachedNode = make(map[*Node]*Node, 0)

	return deepCopy(head)
}

func deepCopy(head *Node) *Node {
	if head == nil {
		return head
	}

	if n, ok := cachedNode[head]; ok {
		return n
	}

	newHead := &Node{Val: head.Val}
	cachedNode[head] = newHead
	newHead.Next = deepCopy(head.Next)
	newHead.Random = deepCopy(head.Random)
	return newHead
}

```


# [Leetcode 141](https://leetcode-cn.com/problems/linked-list-cycle/)
## 1. 题目要求
给你一个链表的头节点 head ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：pos 不作为参数进行传递 。仅仅是为了标识链表的实际情况。

如果链表中存在环 ，则返回 true 。 否则，返回 false 。


## 2. 解法

```
func hasCycle(head *ListNode) bool {
    var slow, fast *ListNode

    slow = head
    fast = head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
        if slow == fast {
            return true
        }
    }
    return false
}
```



# [Leetcode 142](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
## 1. 题目要求
给定一个链表的头节点  head ，**返回链表开始入环的第一个节点**。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。

注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。


## 2. 解法
- 快慢指针检测环
- 数学题，证明 入环距离A = 快慢指针出环点C

```
func detectCycle(head *ListNode) *ListNode {
    slow, fast := head, head

    hasCycle := false
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next

        if fast == slow {
            hasCycle = true
            break
        }
    }

    if hasCycle {
        start := head 
        for start != slow {
            slow = slow.Next
            start = start.Next
        }
        return start
    }

    return nil
}
```


# [Leetcode 143](https://leetcode-cn.com/problems/reorder-list/)
## 1. 题目要求
给定一个单链表 L 的头节点 head ，单链表 L 表示为：

L0 → L1 → … → Ln - 1 → Ln
请将其重新排列后变为：

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

## 2. 解法

```
func middleNode(head *ListNode) *ListNode {
    slow, fast := head, head
    for fast.Next != nil && fast.Next.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    return slow
}

func reverseList(head *ListNode) *ListNode {
    var prev, cur *ListNode = nil, head
    for cur != nil {
        nextTmp := cur.Next
        cur.Next = prev
        prev = cur
        cur = nextTmp
    }
    return prev
}

func mergeList(l1, l2 *ListNode) {
    var l1Tmp, l2Tmp *ListNode
    for l1 != nil && l2 != nil {
        l1Tmp = l1.Next
        l2Tmp = l2.Next

        l1.Next = l2
        l1 = l1Tmp

        l2.Next = l1
        l2 = l2Tmp
    }
}

func reorderList(head *ListNode) {
    if head == nil {
        return
    }
    mid := middleNode(head)
    l1 := head
    l2 := mid.Next
    mid.Next = nil
    l2 = reverseList(l2)
    mergeList(l1, l2)
}
```

# [Leetcode 147](https://leetcode-cn.com/problems/insertion-sort-list/)
## 1. 题目要求
给定单个链表的头 head ，使用 插入排序 对链表进行排序，并返回 排序后链表的头 。

插入排序 算法的步骤:

插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
重复直到所有输入数据插入完为止。

对链表进行插入排序。

O(n^2)

## 2. 解法

```
func insertionSortList(head *ListNode) *ListNode {

}
```

# [Leetcode 148](https://leetcode-cn.com/problems/sort-list/)
## 1. 题目要求
给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。
O(nlogn)


## 2. 解法
1. Solution - top-bottom merge sort
- 找到链表的中点，以中点为分界，将链表拆分成两个子链表。
- 对两个子链表分别排序。

- 将两个排序后的子链表合并，得到完整的排序后的链表。

上述过程可以通过递归实现。递归的终止条件是链表的节点个数小于或等于1。

即当链表为空或者链表只包含 1 个节点时，不需要对链表进行拆分和排序。

- 时间复杂度：O(nlogn)，其中 n 是链表的长度。

- 空间复杂度：O(logn)，其中 n 是链表的长度。空间复杂度主要取决于递归调用的栈空间。

```
func merge(head1, head2 *ListNode) *ListNode {
    dummyHead := &ListNode{}
    temp, temp1, temp2 := dummyHead, head1, head2
    for temp1 != nil && temp2 != nil {
        if temp1.Val <= temp2.Val {
            temp.Next = temp1
            temp1 = temp1.Next
        } else {
            temp.Next = temp2
            temp2 = temp2.Next
        }
        temp = temp.Next
    }
    if temp1 != nil {
        temp.Next = temp1
    } else if temp2 != nil {
        temp.Next = temp2
    }
    return dummyHead.Next
}

func sort(head, tail *ListNode) *ListNode {
    if head == nil {
        return head
    }

    if head.Next == tail {
        head.Next = nil
        return head
    }

    slow, fast := head, head
    for fast != tail {
        slow = slow.Next
        fast = fast.Next
        if fast != tail {
            fast = fast.Next
        }
    }

    mid := slow
    return merge(sort(head, mid), sort(mid, tail))
}

func sortList(head *ListNode) *ListNode {
    return sort(head, nil)
}

```

2. Solution - bottom-up

使用自底向上的方法实现归并排序，则可以达到 O(1) 的空间复杂度。

首先求得链表的长度，然后将链表拆分成子链表进行合并。

具体做法如下。

- 用 subLength 表示每次需要排序的子链表的长度，初始时 subLength=1。

- 每次将链表拆分成若干个长度为subLength 的子链表（最后一个子链表的长度可以小于subLength），按照每两个子链表一组进行合并，合并后即可得到若干个长度为 subLength×2 的有序子链表。

- 将 subLength 的值加倍，重复第 2 步，对更长的有序子链表进行合并操作，直到有序子链表的长度大于或等于length，整个链表排序完毕。

```
func merge(head1, head2 *ListNode) *ListNode {
    dummyHead := &ListNode{}
    temp, temp1, temp2 := dummyHead, head1, head2
    for temp1 != nil && temp2 != nil {
        if temp1.Val <= temp2.Val {
            temp.Next = temp1
            temp1 = temp1.Next
        } else {
            temp.Next = temp2
            temp2 = temp2.Next
        }
        temp = temp.Next
    }
    if temp1 != nil {
        temp.Next = temp1
    } else if temp2 != nil {
        temp.Next = temp2
    }
    return dummyHead.Next
}

func sortList(head *ListNode) *ListNode {
    if head == nil {
        return head
    }

    length := 0
    for node := head; node != nil; node = node.Next {
        length++
    }

    dummyHead := &ListNode{Next: head}
    for subLength := 1; subLength < length; subLength <<= 1 {
        prev, cur := dummyHead, dummyHead.Next
        for cur != nil {
            head1 := cur
            for i := 1; i < subLength && cur.Next != nil; i++ {
                cur = cur.Next
            }

            head2 := cur.Next
            cur.Next = nil
            cur = head2
            for i := 1; i < subLength && cur != nil && cur.Next != nil; i++ {
                cur = cur.Next
            }

            var next *ListNode
            if cur != nil {
                next = cur.Next
                cur.Next = nil
            }

            prev.Next = merge(head1, head2)

            for prev.Next != nil {
                prev = prev.Next
            }
            cur = next
        }
    }
    return dummyHead.Next
}

```


# [Leetcode 206](https://leetcode-cn.com/problems/reverse-linked-list/)
## 1. 题目要求
反转链表

## 2. 解法

```
func reverseList(n *ListNode) *ListNode {
    if n == nil {
        return n
    }

    var pre, curr, next *ListNode
    curr = n
    for curr != nil {
        next = curr.Next
        curr.Next = pre

        pre = curr
        curr = next
    }

    n = pre
    return n
}
```


# [Leetcode 876](https://leetcode-cn.com/problems/middle-of-the-linked-list/)
## 1. 题目要求
给定一个头结点为 head 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

## 2. 解法

```
func middleNode(head *ListNode) *ListNode {
    if head == nil {
		return head
	}
	fast, slow := head, head
	for fast != nil && fast.Next != nil {
        slow = slow.Next
		fast = fast.Next.Next
	}

	return slow
}

```



# [Leetcode 234](https://leetcode-cn.com/problems/palindrome-linked-list/)
## 1. 题目要求
给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。
输入：head = [1,2,2,1]
输出：true

## 2. 解法
切一半
后半reverse
对比两段

```
```



# [Leetcode ]()
## 1. 题目要求


## 2. 解法

```
```

