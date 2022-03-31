## 剑指 Offer 06. 从尾到头打印链表

#### 输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。



---

示例:
```
输入：head = [1,3,2]
输出：[2,3,1]
```

限制：


    0 <= 链表长度 <= 10000

### 我的解答

思路：

	1.递归方式打印，next==nul时返回
	2.用存储结构存起来val，可以用栈或者数组等
    
```go

/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

// 辅助栈，都丢进栈后都pop出来即可
func reversePrint(head *ListNode) []int {
    ans := []int{}
    stack := list.New()
    for cur := head; cur!=nil; cur=cur.Next{
        stack.PushBack(cur.Val)
    }

    for back := stack.Back(); back!=nil ; back=back.Prev(){
        ans = append(ans,back.Value.(int))
    }

    return ans
}

/* 递归方式
func reversePrint(head *ListNode) []int {
    if head == nil {
        return nil
    }
    ans := &[]int{}
    fn(head,ans)
    return *ans
}

func fn(cur *ListNode,ans *[]int){
    if cur == nil {
        return
    }

    fn(cur.Next,ans)
    *ans = append(*ans,cur.Val)
}
*/

```
