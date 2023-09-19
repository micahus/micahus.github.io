---
title: "面试记录2023-09-19"
date: 2023-09-19T16:29:57+08:00
tags: ["Algorithm"]
categories: ["Learning", "Reflections"]
---

到现在面试经历过很多次了，却很少有成功，之前拒掉了几次面试，现在非常后悔。

这里记录下之前面试的他们的算法题

## 1. 如何判断一个链表有环

1. 快慢指针可以解决这个问题

> 使用快慢指针。这里<b>为什么会相遇<b>？最坏的打算当慢指针走一圈的时候，快指针可以走两圈，所以刚好会在同一个点上面。
>
> 如果最后指向null，则说明没有环，如果最后走到了相同点，则说明有环。

2. 如何判断该环的起点在什么地方

![image-20230919163443270](https://image.shijinping.cn/picgo/202309191634061.png)

> 1. 可以假设 环起点 到相遇点的距离为 m
> 2. 那么head到 环起点 刚好是 k-m
> 3. 因为慢指针走了 k 步快指针走了 2k 步相遇了，那么相遇点到快指针的相遇点（N圈之后的相遇点）距离就变成了 2k-k=k 距离一样。那么剪掉相同的环起点到相遇点的 m 都变成了 k-m 步。这样，把任何一个指针的头指针指向 head，用相同的速度，再次相遇点，就是环起点。

## 2. 两条链表是否相交

要判断两个链表是否相交，可以判断有没有共同部分，那么共同部分怎么判断呢？

1. 最原始的办法：使用map（映射）来记录每个node的信息，java中地址值不存在，可以直接判断引用是否一致。golang可以根据地址是否一致，也可以记录value是否一致（当然存在相同的value的情况就不行了）
2. 使用特殊手法，如下图

![image-20230919170716775](https://image.shijinping.cn/picgo/202309191707494.png)

> 1. 双指针同时进行
> 2. 迭代A链表，结束后，迭代B链表
> 3. 迭代B链表，结束后，迭代A链表
> 4. 在迭代的时候，判断两个指针是否一致，如果存在一致

具体代码可以是

```go
	p1, p2 := headA, headB
	for p1 != p2 {
		if p1 == nil {
			p1 = headB
		} else {
			p1 = p1.Next

		}
		if p2 == nil {
			p2 = headA
		} else {
			p2 = p2.Next
		}
	}
	return p1
```

这里有个问题，如果一直没有重复的，会怎么样呢？

> 这个问题比较好回答，因为 A+B 和 B+A 之后，那么他们的总长度是相同的，相同的总长度，如果没有重合的话，肯定是nil，那么肯定满足 p1 = p2 的条件。

## 3. 合并多个链表

合并两个链表可以在程序中，对比两个链表的val来进行合并，但是合并多个链表，就存在不一样的问题了，所以需要其他的处理方式。

所以这里需要用到了priority队列，所以首先要完成优先队列

```go
type Queue []*ListNode

func (q Queue) Len() int{
  return len(q)
}

func (q Queue)Less(i,j int) bool {
  return q[i].Val < q[j].Val
}

func (q Queue)Swap(i, j int){
  q[i],q[j] = q[j], q[i]
}

func (q *Queue) Push(v interface{}){
  *q = append(*q, v.(*LinkNode))
}

func (q *Queue) Pop() interface{} {
  old:= *q
  n:=len(*q)
  v:=*q[n-1]
  *q = old[:n-1]
  return v
}
```

> 这里为什么对Queue的方法，即有Queue对象的，又有Queue指针的呢？
>
> 1. `Queue`本质上是个 *ListNode 数组，其Len/Less/Swap是比较常见的数组用来sort需要定义的函数，
>
> 2. 而Push、Pop则是使用数组来插入、获取的方法
>
> 其中 1 实现的 sort/sort.go中的 `type Interface interface` 接口，所以需要使用的是Queue对象。
>
> 而 2 实现的是对Queue的插入、获取方法，这些方法如果需要对<red>指针对象</red>进行操作，如果不是指针对象，而是复制对象，那不会影响原来的值，就没有意义。
