## 剑指 Offer 30. 包含min函数的栈

#### 定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

---

示例:
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```

提示：

    各函数的调用总次数不超过 20000 次

### 我的解答

思路：

    用切片来当栈用，push直接append，pop时删除最后一个
	再用一个辅助切片来记录递减最小值的下标 
	
	注意：辅助栈此思路也可用到其他类似题型
    
```go

type MinStack struct {
	arr     []int   // 正常栈，append，操作最后一个元素
	descArrIndex []int   // 辅助栈，记录最小值下标，0开始。记录最小值没用，遇到00100没办法处理或困难

    // 如arr=[4,5,6,3,5,3,2]  descArrIndex则记录=[0,3,6]
}

/** initialize your data structure here. */
func Constructor() MinStack {
	return MinStack{
		arr:     []int{},
		descArrIndex: []int{},
	}
}

func (this *MinStack) Push(x int) {
	this.arr = append(this.arr, x)
    // 如果descArr没有元素则直接加入，如果当前最小值>x，则x加入descArr
    descLastIdx := len(this.descArrIndex)-1
    arrLastIdx := len(this.arr)-1
	if len(this.descArrIndex) == 0 || this.arr[this.descArrIndex[descLastIdx]] > x {
		this.descArrIndex = append(this.descArrIndex,arrLastIdx)
	}
}

func (this *MinStack) Pop() {
	if len(this.arr) == 0 {
		return
	}

	if len(this.arr) == 1 || len(this.arr)-1 == this.descArrIndex[len(this.descArrIndex)-1] {
		this.descArrIndex = this.descArrIndex[:len(this.descArrIndex)-1]
	}

	this.arr = this.arr[:len(this.arr)-1]
}

func (this *MinStack) Top() int {
	last := len(this.arr) - 1
	if last < 0 {
		return -1
	}
	return this.arr[last]
}

func (this *MinStack) Min() int {
	last := len(this.descArrIndex) - 1
	if last < 0 {
		return -1
	}
	return this.arr[this.descArrIndex[last]]
}


/**
 * Your MinStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * obj.Pop();
 * param_3 := obj.Top();
 * param_4 := obj.Min();
 */

```
