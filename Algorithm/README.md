---
description: 山重水复疑无路，柳暗花明又一村
---

# Algorithm

<details>

<summary>链表遍历</summary>

```go
type ListNode struct {
    val int
    next *ListNode
}
```

迭代

```go
func traverse(head *ListNode) {
    for p := head; p != nil; p = p.next {
        ...
    }
}
```

递归

```go
func traverse(head *ListNode) {
    // 前序遍历 head.val
    traverse(head.next);
    // 后序遍历 head.val
}
```

改变当前指针

```go
for head != null {
    ...
    head = head.next
}
```

</details>

<details>

<summary>二叉树遍历</summary>

```go
func traverse(head *ListNode) {
    // 前序遍历 head.val
    traverse(head.next);
    // 后序遍历 head.val
}
```

</details>

