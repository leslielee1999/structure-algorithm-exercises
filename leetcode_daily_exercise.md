# 每日练习

## 2022.01.05

### 1. [用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

题目描述：

```
//用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的
//功能。(若队列中没有元素，deleteHead 操作返回 -1 ) 
//
// 
//
// 示例 1： 
//
// 输入：
//["CQueue","appendTail","deleteHead","deleteHead"]
//[[],[3],[],[]]
//输出：[null,null,3,-1]
// 
//
// 示例 2： 
//
// 输入：
//["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
//[[],[],[5],[2],[],[]]
//输出：[null,-1,null,null,5,2]
// 
//
// 提示： 
//
// 
// 1 <= values <= 10000 
// 最多会对 appendTail、deleteHead 进行 10000 次调用 
// 
// Related Topics 栈 设计 队列 👍 395 👎 0
```

解法：

解决的前提是熟悉栈和队列的特性，栈的特性是先进后出，队列的特性是先进先出，而数据的操纵只有插入和删除两种，所以可以用一个栈 `stack1` 作为插入的容器，用另一个栈 `stack2` 作为辅助容器，完成删除操作。

```java
//leetcode submit region begin(Prohibit modification and deletion)
class CQueue {

    Deque<Integer> stack1;
    Deque<Integer> stack2;

    public CQueue() {
        stack1 = new LinkedList<>();
        stack2 = new LinkedList<>();
    }
    
    public void appendTail(int value) {
        stack1.addLast(value);
    }
    
    public int deleteHead() {
        if(stack2.isEmpty()){
            if(stack1.isEmpty()){
                return -1;
            }
            while (!stack1.isEmpty()){
                stack2.addLast(stack1.pop());
            }
            return stack2.pop();
        }else{
            return stack2.pop();
        }
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
//leetcode submit region end(Prohibit modification and deletion)
```

复盘：

第一次完成时的想法是，如果发生删除操作，就把 `stack1` 的数据都移到 `stack2` 里，然后再在 `stack2` 里 `pop` ，再把 `stack2` 里的数据移回 `stack1`，插入操作保持不变，还是只往 `stack1` 里插。这个实现的效率明显比较低，提升效率的方法是，如果发生删除，只要 `stack2` 里有数据，直接 `pop` 即可，具体实现见代码，不难理解。

### 2. [ 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

题目描述：

```
//定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。 
//
// 
//
// 示例: 
//
// MinStack minStack = new MinStack();
//minStack.push(-2);
//minStack.push(0);
//minStack.push(-3);
//minStack.min();   --> 返回 -3.
//minStack.pop();
//minStack.top();      --> 返回 0.
//minStack.min();   --> 返回 -2.
// 
//
// 
//
// 提示： 
//
// 
// 各函数的调用总次数不超过 20000 次 
// 
//
// 
//
// 注意：本题与主站 155 题相同：https://leetcode-cn.com/problems/min-stack/ 
// Related Topics 栈 设计 👍 256 👎 0
```

解法：

选择自定义栈这个数据结构，用一个类来封装，注意，在插入时选择的是【头插法】来添加数据，这样在处理删除操作时比较方便，不用去记录前驱节点来完成删除。

```java
//leetcode submit region begin(Prohibit modification and deletion)
class MinStack {

    class Node{
        int val;
        int min;
        Node next;

        Node(int x, int min){
            this.val = x;
            this.min = min;
            next = null;
        }
    }

    Node head;

    /** initialize your data structure here. */
    public MinStack() {
    }

    //  使用头插法，好处在于处理删除操作时不用去保存前驱节点
    public void push(int x) {
        if(head == null){
            head = new Node(x, x);
        }else{
            Node n = new Node(x, Math.min(x, head.min));
            n.next = head;
            head = n;
        }
    }
    
    public void pop() {
        if (head != null){
            head = head.next;
        }
    }
    
    public int top() {
        return head.val;
    }
    
    public int min() {
        return head.min;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
//leetcode submit region end(Prohibit modification and deletion)
```

复盘：

像这种叫我们自定义数据结构的题目，最好一开始就想到用一个类去封装，既符合题目要求，也不会走弯路，想得更复杂。