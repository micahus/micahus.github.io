---
title: "链表转换"
date: 2023-09-20T07:53:39+08:00
tags: ["Algorithm"]
categories: ["Learning", "Reflections"]
---

## 链表反转

[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

### for循环进行处理

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

