## 剑指 Offer 35. 复杂链表的复制

#### 请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。



---

示例 1：
!["示例1"](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```


示例 2：
!["示例2"](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)

```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```

示例 3：
!["示例3"](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)

```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```

示例 4：
```
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。
```

提示：

* -10000 <= Node.val <= 10000
* Node.random 为空（null）或指向链表中的节点。
* 节点数目不超过 1000 。

注意：本题与主站 138 题相同：[leetcode-138](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

### 我的解答

思路：

	copy全部新节点，然后有旧新结点关系old2NewRelation map[oldNode]newNode关系
    遍历原始节点，利用map[oldNode]newNode关系即可还原结构
    old2NewRelation[cur].Next = old2NewRelation[cur.Next]
    old2NewRelation[cur].Random = old2NewRelation[cur.Random]
    
```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Next *Node
 *     Random *Node
 * }
 */

 // 新思路：copy全部新节点，然后有旧新结点关系old2NewRelation map[oldNode]newNode关系
 //       遍历原始节点，利用map[oldNode]newNode关系即可还原结构
func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }

    old2NewRelation := map[*Node]*Node{}
    cur := head
    // 节点复制 和 建立 旧新节点关系map
    for cur != nil {
       item := &Node{
           Val: cur.Val,
       }

        old2NewRelation[cur] = item
        cur = cur.Next
    }

    // 给新节点的Next,Random赋值
    cur = head
    for cur != nil {
        old2NewRelation[cur].Next = old2NewRelation[cur.Next]
        old2NewRelation[cur].Random = old2NewRelation[cur.Random]
        cur = cur.Next
    }

    return old2NewRelation[head]
}


// 第一次思路：遍历原先节点，复制份新的Node{Val}存到切片里，之后再遍历新的，补充上Next
//       random关系因为对象变了，所以要记录关系map[index]index才有效
//       遍历原先节点的时候存srcNode2RandomNode map[*Node]*Node
//                      存节点下标 srcNodeIndex := map[*Node]int{}
//      --->	relataionN2RIndex := map[int]int{} ，再把这个关系复现到新节点上即可  

// 上面的思路不好，请看题解思路，下次去实现
//  todo 新旧Node对应关系 一个map，即可实现
 // todo 旧对象后面接新对象，更新random后，再拆分
 //             Node1->newNode1->Node2->newNode2->Node...->newNode...->nil
 /*
func copyRandomList(head *Node) *Node {
	if head == nil {
		return nil
	}
	// 需要把A节点到具体哪个random的节点的下标记录下
	// 记录所有节点下拨信息，
	// map[nodeA]NodeB, map[node]index --> map[index]index
	srcNode2RandomNode := map[*Node]*Node{}
	srcNodeIndex := map[*Node]int{}
	relataionN2RIndex := map[int]int{}
	newNodeList := []*Node{}
	i := 0
	for cur := head; cur != nil; cur = cur.Next {
		srcNode2RandomNode[cur] = cur.Random
		srcNodeIndex[cur] = i
		i++

		newNodeList = append(newNodeList, &Node{
			Val: cur.Val,
		})
	}

	for n, toRandom := range srcNode2RandomNode {
		if toRandom == nil {
			relataionN2RIndex[srcNodeIndex[n]] = -1
			continue
		}
		relataionN2RIndex[srcNodeIndex[n]] = srcNodeIndex[toRandom]
	}

	for i := 0; i < len(newNodeList); i++ {
		if i == len(newNodeList)-1 {
			newNodeList[i].Next = nil
			break
		}
		newNodeList[i].Next = newNodeList[i+1]
	}

	for curI, toRandJ := range relataionN2RIndex {
		if toRandJ == -1 {
			newNodeList[curI].Random = nil
			continue
		}
		newNodeList[curI].Random = newNodeList[toRandJ]
	}

	return newNodeList[0]
}
*/

```
