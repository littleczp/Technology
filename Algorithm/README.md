---
description: 山重水复疑无路，柳暗花明又一村
---

# Algorithm

## 框架

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

<summary>树遍历</summary>

```go
type TreeNode struct {
    val int
    left, right *TreeNode
}
```

二叉树

```go
func traverse(root *TreeNode) {
    // 前序遍历
    traverse(root.left);
    // 中序遍历
    traverse(root.right);
    // 后序遍历
}

```

N叉树

```go
type TreeNode struct {
    val      int
    children []*TreeNode
}

func traverse(root *TreeNode) {
    for _, child := range root.children {
        traverse(child)
    }
}
```

</details>

