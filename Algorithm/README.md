---
description: 山重水复疑无路，柳暗花明又一村
---

# Algorithm

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
