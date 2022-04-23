
---
layout: post
title: "Golang Tools"
date: 2022-05-10
---

Golang 原生支持数据结构较少，不便于刷题，在此放一些辅助的api

#Priority Queue

需要使用原生heap库实现PQ（修改比较器Less, Swap, Len 和 Push Pop函数）.
先计数，再push到pq内，最后输val前k个的key。


```
type Item struct {
	Key   int
	Count int
}

// A PriorityQueue implements heap.Interface and holds Items.
type PriorityQueue struct {
	pq []*Item
}

func (pq *PriorityQueue) Len() int {
	return len(pq.pq)
}

func (pq *PriorityQueue) Less(i, j int) bool {
	return pq.pq[i].Count > pq.pq[j].Count // 大数在前
}

func (pq *PriorityQueue) Swap(i, j int) {
	pq.pq[i], pq.pq[j] = pq.pq[j], pq.pq[i]
}

// Push define
func (pq *PriorityQueue) Push(x interface{}) {
	item := x.(*Item)
	pq.pq = append(pq.pq, item)
}

// Pop define
func (pq *PriorityQueue) Pop() interface{} {
	if len(pq.pq) == 0 {
		return nil
	}
	n := len(pq.pq)
	item := (pq.pq)[n-1]
	pq.pq = (pq.pq)[:n-1]
	return item
}

```

Queue

```
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

Union Find

```
```
