---
description: 山重水复疑无路，柳暗花明又一村
---

# Algorithm

## 结构

{% tabs %}
{% tab title="链表" %}
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


{% endtab %}

{% tab title="树" %}
二叉树

<pre class="language-go"><code class="lang-go">type TreeNode struct {
    val int
    left, right *TreeNode
}

<strong>func traverse(root *TreeNode) {
</strong>    // 前序遍历
    traverse(root.left);
    // 中序遍历
    traverse(root.right);
    // 后序遍历
}

</code></pre>

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
{% endtab %}
{% endtabs %}

## 框架

{% tabs %}
{% tab title="回溯" %}
```python
result = []
def backtrack(path, choice):
    if ...:
        result.append(path)
        return
    for c in choice:
        选择
        backtrack(paht, choice)
        撤销选择
```
{% endtab %}
{% endtabs %}
