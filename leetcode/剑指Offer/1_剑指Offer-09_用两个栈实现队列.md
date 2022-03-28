## 剑指 Offer 09. 用两个栈实现队列

#### 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

---

示例 1：
```
输入：
    ["CQueue","appendTail","deleteHead","deleteHead"]
    [[],[3],[],[]]

输出：[null,null,3,-1]
```

示例 2：
```
输入：
    ["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
    [[],[],[5],[2],[],[]]

输出：[null,-1,null,null,5,2]
```
提示：

    1 <= values <= 10000
    最多会对 appendTail、deleteHead 进行 10000 次调用

### 我的解答

思路：

    输入都由in来接收，需要输出时push in的所有元素到out，此时有序
    如果out里有元素，则直接pop元素
    如果out没有元素了，则把in里的所有元素都push进out栈，此时out有序
    
```go
type CQueue struct {
    in  *list.List  // 接收输入
    out *list.List  // 顺序输出
    
}


func Constructor() CQueue {
	return CQueue{in: list.New(), out: list.New()}
}

// 直接In接收
func (this *CQueue) AppendTail(value int)  {
	this.in.PushBack(value)
}

//     如果out里有元素，则直接pop元素
//     如果out没有元素了，则把in里的所有元素都push进out栈，此时out有序
func (this *CQueue) DeleteHead() int {
    outEle := this.out.Back()
	if outEle != nil {
		this.out.Remove(outEle)
		return outEle.Value.(int)
	}

	inEle := this.in.Back()
	for inEle != nil {
		this.out.PushBack(inEle.Value)
		this.in.Remove(inEle)
		inEle = this.in.Back()
	}

	outEle = this.out.Back()
	if outEle==nil {
		return -1
	}
	
	this.out.Remove(outEle)
	return outEle.Value.(int)
}


/**
 * Your CQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AppendTail(value);
 * param_2 := obj.DeleteHead();
 */

```
