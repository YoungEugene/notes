## 剑指 Offer 24. 反转链表

#### 定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。


---

示例:
```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

限制：

    0 <= 节点个数 <= 5000


### 我的解答

思路：

	记录好pre，遍历所有节点，当前节点cur.next都修改为pre节点
    
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    var pre,cur *ListNode = nil,head
    for cur != nil {
        next := cur.Next
        cur.Next = pre
        pre = cur
        cur = next
    }
    return pre
}

```
