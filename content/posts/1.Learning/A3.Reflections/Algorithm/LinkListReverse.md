---
title: "链表转换"
date: 2023-09-20T07:53:39+08:00
tags: ["Algorithm"]
categories: ["Learning", "Reflections"]
---

这里没有记住，需要在吸收下

## 链表反转

[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

### for循环处理

时间复杂度O(N)（循环整个链表）,空间复杂度O(1)，只有几个临时变量

<img src="https://image.shijinping.cn/picgo/202309200854577.png" alt="image-20230920085359962" style="zoom:50%;" />

其实链表反转，无非就是上面这张图。

1. 记录current的next节点（因为这里current的next需要指向pre）
2. 把current的next指向pre
3. 把current节点变成pre节点
4. 把next节点变成current节点

> 最后当current节点变成空时，pre节点就是反转后的链表

代码如下：

```go
	if head == nil || head.Next == nil {
		return head
	}
	current := head // 把头指向当前
	var pre *ListNode // 做一个pre节点
	for current != nil {
		next := current.Next // 1.记录当前的下一个节点
		current.Next = pre // 2.把当前的next指向pre
		pre = current // 3.把current节点变成pre节点
		current = next // 4.把next节点变成current节点
	}
	return pre
```

### 迭代处理

![image-20230920092045168](https://image.shijinping.cn/picgo/202309200920953.png)

核心思想：“2”节点后面的额所有元素都进过反转了，但是head“1”节点的next还是指向了“2”，所以可以直接把“2”的next指向head，这样就有反转后的链表了

```go
if head == nil || head.Next == nil {
  return head
}

last := reverse(head.Next)
head.Next.Next = head
head.Next = nil
return last
```

## 反转链表中间部分

[92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)

### for 循环处理

![image-20230920093054992](https://image.shijinping.cn/picgo/202309200930649.png)

这里分成三部分：

1. 前段：不需要反转的
2. 中段：需要反转的
3. 后段：不需要反转的

如果这里left是0，那么就会出现前段为空，那么一般情况是加上dummy节点来处理。步骤分别是：

1. 把p0的next的next指向current
2. 把p0的next是pre

```go
	if head == nil || head.Next == nil {
		return head
	}

	dummy := &ListNode{Next: head}
	p0 := dummy
	for i := 0; i < left-1; i++ { // 注意：这里需要left-1，而不是left，因为需要少一个
		p0 = p0.Next
	}

	var pre *ListNode
	current := p0.Next

	for i := 0; i < right-left+1; i++ {
		next := current.Next
		current.Next = pre
		pre = current
		current = next
	}
	p0.Next.Next = current // 把p0的next的next指向current
	p0.Next = pre // 把p0的next是pre
	return dummy.Next
```



### 迭代处理

```go
	if head == nil || head.Next == nil {
		return head
	}
	var successor *ListNode

	var reverseN func(head *ListNode, n int) *ListNode
	reverseN = func(head *ListNode, n int) *ListNode {
		if n == 1 {
			successor = head.Next
			return head
		}
		last := reverseN(head.Next, n-1)
		head.Next.Next = head
		head.Next = successor
		return last
	}
	if left == 1 {
		return reverseN(head, right)
	}
	head.Next = reverseBetween(head.Next, left-1, right-1)
	return head
```

### 按照固定的K个长度进行反转

[25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

这题还不会。。。

### 走一半后，反转链表

[234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

#### 方法1，非正规

把链表转换成数组，用数据进行处理

```go
	// 第一种：如果用slice来处理，耗时和内存占用都比较大
	// 	执行耗时:124 ms,击败了50.65% 的Go用户
	//	内存消耗:8.3 MB,击败了73.55% 的Go用户
	s := make([]int, 0, 100000) // 这里cap为100000，因为最大长度是10的5次方
	for head != nil {
		s = append(s, head.Val)
		head = head.Next
	}
	left, right := 0, len(s)-1
	for left < right {
		if s[left] != s[right] {
			return false
		}
		left++
		right--
	}
	return true
```

#### 方法2

> 1. 使用快慢指针获取链表的中间位置
> 2. 反转后半部分
> 3. 反转后的链表和原链表进行匹配

其中需要注意的部分是，怎么判断中间部分

![image-20230921170016832](https://image.shijinping.cn/picgo/202309211700384.png)

这里的奇数部分需要做特殊处理，当fast不是nil，就说明是奇数，那么比对的是`2、1`这两个节点，所以slow需要往后移动一格

```go
	fast, slow := head, head
	for fast != nil && fast.Next != nil {
		fast = fast.Next.Next
		slow = slow.Next
	}
	if fast != nil {
		slow = slow.Next // 这里就是上面图的特殊处理
	}
	left, right := head, reverse(slow) // reverse方法就是普通的反转链表
	for right != nil {
		if left.Val != right.Val {
			return false
		}
		left = left.Next
		right = right.Next
	}
	return true
```

