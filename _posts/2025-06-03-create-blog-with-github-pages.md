---
layout: post
title: "数据结构之 Linked Lists详解"
date: 2025-06-03
tags: [Linked Lists]
comments: true
author: Erato
permalink: /create-blog-with-github-pages/2025-06-03/
---
# LeetCode 例题--代码随想录第二章 链表part01


代码随想录讲义：[链表基础](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E9%93%BE%E8%A1%A8%E7%9A%84%E7%B1%BB%E5%9E%8B)

## 232. 移除链表元素

题目链接：[203.移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/description/)
## 方法一：递归法

### 思路
链表的结构本身具有递归性质，可以递归地处理链表。

- 对当前节点 `head`，先递归处理其后续节点：`head.next = removeElements(head.next, val)`
- 然后判断当前节点的值：
  - 如果 `head.val == val`，则删除当前节点，返回 `head.next`
  - 否则，保留当前节点，返回 `head`

### 代码示例（Java）
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;  // 递归终止条件
        }
        head.next = removeElements(head.next, val);  // 递归处理后续节点
        return head.val == val ? head.next : head;   // 判断当前节点是否删除
    }
}
```
在 Java 中，三元条件运算符的基本结构是：

```java
condition ? expr1 : expr2;
```
如果 condition 为真，表达式的值为 expr1,如果 condition 为假，表达式的值为 expr2
复杂度分析
时间复杂度：O(n)，遍历所有节点一次

空间复杂度：O(n)，递归调用栈最大深度为链表长度

## 方法二：迭代

### 思路
用迭代方式遍历链表，逐个检查节点值是否等于 val，并删除。

创建哑节点 dummyHead，指向 head，方便处理头节点被删除的情况

使用指针 temp 遍历链表

当 temp.next 节点的值等于 val，删除该节点：temp.next = temp.next.next

否则，temp 向前移动：temp = temp.next

遍历结束，返回 dummyHead.next

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode temp = dummyHead;
        while (temp.next != null) {
            if (temp.next.val == val) {
                temp.next = temp.next.next;  // 删除节点
            } else {
                temp = temp.next;  // 移动指针
            }
        }
        return dummyHead.next;
    }
}
```
python 版本
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # 创建虚拟头部节点以简化删除过程
        dummy_head = ListNode(next = head)
        
        # 遍历列表并删除值为val的节点
        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        
        return dummy_head.next
```
复杂度分析
时间复杂度：O(n)，遍历链表一次

空间复杂度：O(1)，只使用了常数级额外空间
### 常见时间复杂度级别（从快到慢）

| 复杂度        | 含义                                 | 例子                   |
|---------------|------------------------------------|------------------------|
| O(1)          | 常数时间，运行时间固定不变           | 访问数组元素           |
| O(log n)      | 对数时间，随着输入规模增长缓慢增加   | 二分查找               |
| O(n)          | 线性时间，运行时间和输入规模成正比   | 单层循环遍历数组       |
| O(n log n)    | 线性对数时间                       | 归并排序、快速排序     |
| O(n²)         | 平方时间，嵌套双层循环               | 冒泡排序、选择排序     |
| O(2^n)        | 指数时间，算法非常慢                 | 递归解决斐波那契数列（朴素法） |
| O(n!)         | 阶乘时间，极慢                     | 旅行商问题暴力解法     |

---

## 什么是空间复杂度？

空间复杂度表示算法运行过程中占用内存空间与输入规模之间的关系，也用大 O 记号表示。  
包括输入数据本身占用空间和算法运行时额外申请的空间。



707：[设计链表](https://leetcode.cn/problems/design-linked-list/description/）

在链表类中实现这些功能：

get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。
单链表
java版本
```java
//单链表
class MyLinkedList {

    class ListNode {
        int val;
        ListNode next;
        ListNode(int val) {
            this.val=val;
        }
    }
    //size存储链表元素的个数
    private int size;
    //注意这里记录的是虚拟头结点
    private ListNode head;

    //初始化链表
    public MyLinkedList() {
        this.size = 0;
        this.head = new ListNode(0);
    }

    //获取第index个节点的数值，注意index是从0开始的，第0个节点就是虚拟头结点
    public int get(int index) {
        //如果index非法，返回-1
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode cur = head;
        //第0个节点是虚拟头节点，所以查找第 index+1 个节点
        for (int i = 0; i <= index; i++) {
            cur = cur.next;
        }
        return cur.val;
    }

    public void addAtHead(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head.next;
        head.next = newNode;
        size++;

        // 在链表最前面插入一个节点，等价于在第0个元素前添加
        // addAtIndex(0, val);
    }

    
    public void addAtTail(int val) {
        ListNode newNode = new ListNode(val);
        ListNode cur = head;
        while (cur.next != null) {
            cur = cur.next;
        }
        cur.next = newNode;
        size++;

        // 在链表的最后插入一个节点，等价于在(末尾+1)个元素前添加
        // addAtIndex(size, val);
    }

    // 在第 index 个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果 index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果 index 大于链表的长度，则返回空
    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size) {
            return;
        }

        //找到要插入节点的前驱
        ListNode pre = head;
        for (int i = 0; i < index; i++) {
            pre = pre.next;
        }
        ListNode newNode = new ListNode(val);
        newNode.next = pre.next;
        pre.next = newNode;
        size++;
    }

    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        
        //因为有虚拟头节点，所以不用对index=0的情况进行特殊处理
        ListNode pre = head;
        for (int i = 0; i < index ; i++) {
            pre = pre.next;
        }
        pre.next = pre.next.next;
        size--;
    }
}
```
python版本
```python
（版本一）单链表法
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
        
class MyLinkedList:
    def __init__(self):
        self.dummy_head = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        
        current = self.dummy_head.next
        for i in range(index):
            current = current.next
            
        return current.val

    def addAtHead(self, val: int) -> None:
        self.dummy_head.next = ListNode(val, self.dummy_head.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        current = self.dummy_head
        while current.next:
            current = current.next
        current.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = ListNode(val, current.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = current.next.next
        self.size -= 1


# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```
双链表

java版本
```java
//双链表
class MyLinkedList {  

    class ListNode{
        int val;
        ListNode next, prev;
        ListNode(int val){
            this.val = val;
        }
    }

    //记录链表中元素的数量
    private int size;
    //记录链表的虚拟头结点和尾结点
    private ListNode head, tail;
    
    public MyLinkedList() {
        //初始化操作
        this.size = 0;
        this.head = new ListNode(0);
        this.tail = new ListNode(0);
        //这一步非常关键，否则在加入头结点的操作中会出现null.next的错误！！！
        this.head.next = tail;
        this.tail.prev = head;
    }
    
    public int get(int index) {
        //判断index是否有效
        if(index < 0 || index >= size){
            return -1;
        }
        ListNode cur = head;
        //判断是哪一边遍历时间更短
        if(index >= size / 2){
            //tail开始
            cur = tail;
            for(int i = 0; i < size - index; i++){
                cur = cur.prev;
            }
        }else{
            for(int i = 0; i <= index; i++){
                cur = cur.next; 
            }
        }
        return cur.val;
    }
    
    public void addAtHead(int val) {
        //等价于在第0个元素前添加
        addAtIndex(0, val);
    }
    
    public void addAtTail(int val) {
        //等价于在最后一个元素(null)前添加
        addAtIndex(size, val);
    }
    
    public void addAtIndex(int index, int val) {
        //判断index是否有效
        if(index < 0 || index > size){
            return;
        }

        //找到前驱
        ListNode pre = head;
        for(int i = 0; i < index; i++){
            pre = pre.next;
        }
        //新建结点
        ListNode newNode = new ListNode(val);
        newNode.next = pre.next;
        pre.next.prev = newNode;
        newNode.prev = pre;
        pre.next = newNode;
        size++;
        
    }
    
    public void deleteAtIndex(int index) {
        //判断index是否有效
        if(index < 0 || index >= size){
            return;
        }

        //删除操作
        ListNode pre = head;
        for(int i = 0; i < index; i++){
            pre = pre.next;
        }
        pre.next.next.prev = pre;
        pre.next = pre.next.next;
        size--;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```
python版本
```python
（版本二）双链表法
class ListNode:
    def __init__(self, val=0, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next

class MyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        
        if index < self.size // 2:
            current = self.head
            for i in range(index):
                current = current.next
        else:
            current = self.tail
            for i in range(self.size - index - 1):
                current = current.prev
                
        return current.val

    def addAtHead(self, val: int) -> None:
        new_node = ListNode(val, None, self.head)
        if self.head:
            self.head.prev = new_node
        else:
            self.tail = new_node
        self.head = new_node
        self.size += 1

    def addAtTail(self, val: int) -> None:
        new_node = ListNode(val, self.tail, None)
        if self.tail:
            self.tail.next = new_node
        else:
            self.head = new_node
        self.tail = new_node
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        
        if index == 0:
            self.addAtHead(val)
        elif index == self.size:
            self.addAtTail(val)
        else:
            if index < self.size // 2:
                current = self.head
                for i in range(index - 1):
                    current = current.next
            else:
                current = self.tail
                for i in range(self.size - index):
                    current = current.prev
            new_node = ListNode(val, current, current.next)
            current.next.prev = new_node
            current.next = new_node
            self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        
        if index == 0:
            self.head = self.head.next
            if self.head:
                self.head.prev = None
            else:
                self.tail = None
        elif index == self.size - 1:
            self.tail = self.tail.prev
            if self.tail:
                self.tail.next = None
            else:
                self.head = None
        else:
            if index < self.size // 2:
                current = self.head
                for i in range(index):
                    current = current.next
            else:
                current = self.tail
                for i in range(self.size - index - 1):
                    current = current.prev
            current.prev.next = current.next
            current.next.prev = current.prev
        self.size -= 1



# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```



 206.[反转链表](https://leetcode.cn/problems/reverse-linked-list/description/)
方法一：双指针法

```java
// 双指针
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        ListNode temp = null;
        while (cur != null) {
            temp = cur.next;// 保存下一个节点
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
}
```
方法二：递归法
```java
class Solution {
    ListNode reverseList(ListNode head) {
        // 边缘条件判断
        if(head == null) return null;
        if (head.next == null) return head;
        
        // 递归调用，翻转第二个节点开始往后的链表
        ListNode last = reverseList(head.next);
        // 翻转头节点与第二个节点的指向
        head.next.next = head;
        // 此时的 head 节点为尾节点，next 需要指向 NULL
        head.next = null;
        return last;
    } 
}
```



24. [两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)
方法一：递归

```java
// 递归版本
class Solution {
    public ListNode swapPairs(ListNode head) {
        // base case 退出提交
        if(head == null || head.next == null) return head;
        // 获取当前节点的下一个节点
        ListNode next = head.next;
        // 进行递归
        ListNode newNode = swapPairs(next.next);
        // 这里进行交换
        next.next = head;
        head.next = newNode;

        return next;
    }
} 
```

python版本
```python
# 递归版本
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        # 待翻转的两个node分别是pre和cur
        pre = head
        cur = head.next
        next = head.next.next
        
        cur.next = pre  # 交换
        pre.next = self.swapPairs(next) # 将以next为head的后续链表两两交换
         
        return cur
```

方法二：虚拟头节点
```java
// 将步骤 2,3 交换顺序，这样不用定义 temp 节点
public ListNode swapPairs(ListNode head) {
    ListNode dummy = new ListNode(0, head);
    ListNode cur = dummy;
    while (cur.next != null && cur.next.next != null) {
        ListNode node1 = cur.next;// 第 1 个节点
        ListNode node2 = cur.next.next;// 第 2 个节点
        cur.next = node2; // 步骤 1
        node1.next = node2.next;// 步骤 3
        node2.next = node1;// 步骤 2
        cur = cur.next.next;
    }
    return dummy.next;
}
```
python版本
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy_head = ListNode(next=head)
        current = dummy_head
        
        # 必须有cur的下一个和下下个才能交换，否则说明已经交换结束了
        while current.next and current.next.next:
            temp = current.next # 防止节点修改
            temp1 = current.next.next.next
            
            current.next = current.next.next
            current.next.next = temp
            temp.next = temp1
            current = current.next.next
        return dummy_head.next
```





# Linked Lists（链表）

## 1. 什么是链表（Linked List）

- 链表是一种由一系列节点组成的数据结构，每个节点包含数据和指向下一个（或上一个）节点的指针。
- 节点是动态创建和销毁的：新增元素时创建新节点并链接，删除元素时销毁对应节点。
- 链表有一个头指针指向第一个节点，尾指针是可选的。

---

## 2. 链表与数组的区别

- 链表更适合数据的动态操作（插入和删除），而数组更适合随机访问。
- 链表不需要预定义容量，大小动态变化。

---

## 3. 单链表（Singly Linked List）

- 每个节点只有一个指针，指向下一个节点，无法回溯到前一个节点。
- 链表从头指针开始遍历。
- 每个节点包含两个部分：
  - **元素数据**（element）
  - **指向下一个节点的指针**（next）

示意：
![Singly Linked List](/images/linkedlist1.png)


- 最后一个节点的 `next` 指向 `null`，表示链表结束。

---
单链表的变化--增删查

```java
class MyLinkedList {
    class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }
    private int size;
    private ListNode head;           // 虚拟头结点

    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }
}
```
### 1.在头部插入节点（Insert at Head）

步骤：
假设链表里已经有三个节点 A → B → C，并且我们有一个虚拟头结点 head：
```css
head ──► A ──► B ──► C ──► null
```
第 1 步：newNode.next = head.next
head.next 当前指向 A（原来的首节点）。

把这个地址拷贝给 newNode.next，意思是：

“让 newNode 的 next 指针也指向 A”。

执行后，链表有两条引用指向 A：
```css
head ──► A ──► B ──► C ──► null
          ▲
newNode───┘
```
✅ 没动 head 指针，只是给 newNode 安排好“接班人”。

第 2 步：head.next = newNode
现在把虚拟头 head 的 next 指向 newNode，相当于把链表入口改成了 newNode。

执行后
```css
head ──► newNode ──► A ──► B ──► C ──► null
```
✅ 链表入口已更新，整个链表仍然连通，没有节点丢失。

code:
```java
    public void addAtHead(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head.next;
        head.next = newNode;
        size++;
    }
```

---

### 2. 在尾部插入节点（Insert at Tail）

步骤：

插入前的链表
假设链表已有三个节点 A → B → C，head 是虚拟头结点：

```css
head ──► A ──► B ──► C ──► null   （C 是当前尾节点）
```
第 1 步：cur.next = newNode
cur 此时就是 原尾节点 C。
把 C 的 next 指针改成 newNode，等于“把新节点接到最后”。

执行后效果图：

```css
head ──► A ──► B ──► C ──► newNode
                                      ▲
                                      │
                                   newNode.next
```
C 不再指向 null，而是指向了 newNode。

其他部分（A、B 的指针）都没动，链表仍保持连通。

第 2 步：newNode.next == null
newNode 创建时构造函数里已经把 next 设为 null，表示“我暂时是最后一个”。

因此 无需再写一句 newNode.next = null;，逻辑上它已经是尾巴。

最终链表：

```css
head ──► A ──► B ──► C ──► newNode ──► null
```
✅ 链表长度 +1，新节点安全成为尾节点，没有任何旧节点丢失。



code:
```java
    public void addAtTail(int val) {
        ListNode newNode = new ListNode(val);
        ListNode cur = head;
        while (cur.next != null) {
            cur = cur.next;
        }
        cur.next = newNode;
        size++;
    }
```


---

### 3. 删除头节点（Remove at Head）

步骤：

1. 将头指针指向当前头节点的下一个节点。
2. 旧头节点等待垃圾回收。

code：

```java
    public void removeAtHead() {
        if (size == 0) return;          // 空链表，无需删除
        head.next = head.next.next;     // 删除首节点
        size--;
    }
```


---

### 4. 删除尾节点（Remove at Tail）

- 单链表中删除尾节点不高效。
- 需要遍历找到倒数第二个节点。
- 更新尾指针（如果存在）指向倒数第二个节点。
- 将倒数第二个节点的 `next` 置为 `null`。
code:

```java
    public void removeAtTail() {
        if (size == 0) return;              // 空链表
        ListNode prev = head;
        // 找到倒数第二个节点
        while (prev.next != null && prev.next.next != null) {
            prev = prev.next;
        }
        prev.next = null;                   // 删除尾节点
        size--;
    }
```
---

### 5. 在任意位置插入或删除节点

- 可以在链表的任意位置插入或删除节点。
- 需更新相关节点的 `next` 指针（以及双链表中的 `prev` 指针）以保证链表结构完整且可遍历。

插入
code:
```java
    public void addAtIndex(int index, int val) {
        if (index > size) return;           // 超界，无效
        if (index < 0) index = 0;           // 负数视作 0

        ListNode prev = head;
        for (int i = 0; i < index; i++) {
            prev = prev.next;
        }
        ListNode newNode = new ListNode(val);
        newNode.next = prev.next;
        prev.next = newNode;
        size++;
    }

```

删除
code：
```java
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) return;  // 越界
        ListNode prev = head;
        for (int i = 0; i < index; i++) {
            prev = prev.next;
        }
        prev.next = prev.next.next;              // 删除节点
        size--;
    }
```
---


### 6. 获取指定下标元素（Get）
```java
    public int get(int index) {
        if (index < 0 || index >= size) return -1;
        ListNode cur = head.next;
        for (int i = 0; i < index; i++) {
            cur = cur.next;
        }
        return cur.val;
    }
```

# Iterators（迭代器）

## 1. 迭代器的背景

- 数据结构种类繁多：数组、数组列表、链表、各种树、哈希表等。
- 一个常见需求是遍历数据结构中的所有元素。
- 但不同数据结构遍历方式不同，通常需要为每种结构写不同代码。
- 迭代器抽象了这一过程，提供统一接口遍历各种数据结构。

---

## 2. 迭代器的定义和作用

- 迭代器通常是一个特殊的类或对象，用于遍历数据结构。
- 它封装了具体的数据访问细节，提供通用的操作方法。
- 迭代器常见方法包括：
  - 获取当前元素
  - 移动到下一个元素
  - （可选）移动到上一个元素

---

## 3. Java 中的迭代器接口

- Java `java.util.Iterator` 接口主要包含三个方法：
  - `hasNext()`：判断是否还有下一个元素。
  - `next()`：返回下一个元素。
  - `remove()`：移除最近返回的元素（可选实现）。
- `remove()` 方法不是强制实现，Java 8 起默认抛出 `UnsupportedOperationException`。

---

## 4. 迭代器的实现方式

- 数据结构一般会内部实现迭代器，常见为内部类或私有类。
- 例如，Java 中链表会实现一个内部类，实现 `Iterator` 接口来遍历链表元素。
- 数据结构类自身实现 `Iterable` 接口，提供 `iterator()` 方法返回对应迭代器实例。

---

## 5. 迭代器的使用

- 用户通过调用数据结构的 `iterator()` 方法获得迭代器。
- 然后通过循环调用 `hasNext()` 和 `next()` 遍历所有元素。
- 注意：数据结构在遍历过程中若发生修改，可能导致迭代器失效并抛出异常（如 `ConcurrentModificationException`）。

---

## 6. Java 迭代器示例代码（单链表）

```java
public class LinkedList<T> implements Iterable<T> {
    private Node<T> head;

    // 省略其他方法

    @Override
    public Iterator<T> iterator() {
        return new LinkedListIterator<>(head);
    }

    private static class LinkedListIterator<T> implements Iterator<T> {
        private Node<T> currentNode;

        public LinkedListIterator(Node<T> startNode) {
            this.currentNode = startNode;
        }

        @Override
        public boolean hasNext() {
            return currentNode != null;
        }

        @Override
        public T next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }
            T data = currentNode.data;
            currentNode = currentNode.next;
            return data;
        }

        @Override
        public void remove() {
            throw new UnsupportedOperationException();
        }
    }

    private static class Node<T> {
        T data;
        Node<T> next;
        Node(T data) { this.data = data; }
    }
}
```
---

## 7. 使用迭代器遍历示例

LinkedList<Integer> linkedList = new LinkedList<>();
// 假设链表已添加元素

Iterator<Integer> it = linkedList.iterator();
while (it.hasNext()) {
    Integer data = it.next();
    System.out.println("Data: " + data);
}
---

## 8. Java 的增强 for 循环（for-each）
Java 支持用增强 for 循环遍历实现了 Iterable 接口的数据结构。

语法更简洁，减少出错概率。

```
for (Integer data : linkedList) {
    System.out.println("Data: " + data);
}
```
---

## 9. 迭代器的性能
迭代器通常具有与其他访问方法相同或更优的时间复杂度。

例如对链表，迭代器的遍历为 O(n)，而调用 get() 访问每个元素为 O(n²)。

---

## 10. 多种迭代器与应用
数据结构可实现多种迭代器，例如：

列表的正向迭代器与反向迭代器。

树的前序、中序、后序、层序迭代器等（稍后学习树结构时详解）。

但一个数据结构的 iterator() 方法只能返回一种迭代器，供 for-each 循环使用。

迭代器为不同数据结构提供了统一的遍历接口，简化代码，提升可维护性，是现代编程语言重要的设计模式之一。

# Linked Lists（链表）

## 1. 链表回顾（Recall Linked Lists）

- 链表是一种由节点组成的数据结构，每个节点包含数据及指向下一个/上一个节点的指针。
- 节点动态创建和销毁：添加新元素时创建节点并连接，删除元素时销毁对应节点。
- 链表至少有一个头指针指向第一个节点，尾指针可选。

---

## 2. 单链表回顾（Recall Singly Linked Lists）

- 单链表不使用“虚拟”节点，最后一个节点的 `next` 指向 `null`。
- 结构示意：

data → data → data → data → data → null
head

yaml
Copy

---

## 3. 双链表（Doubly Linked List）

- 每个节点含有两个指针：一个指向下一个节点，一个指向前一个节点。
- 可双向遍历链表（向前与向后）。
- 节点包含：
  - 元素（element）
  - 指向前节点的指针（prev）
  - 指向后节点的指针（next）

结构示意：

element
prev <--> next

kotlin
Copy

---

## 4. 双链表节点的泛型代码示例（Java）

```java
public class DoublyLinkedList<Type> {
    private class Node<Type> {
        private Type data;
        private Node<Type> next;
        private Node<Type> prev;

        private Node(Type data, Node<Type> next, Node<Type> prev) {
            this.data = data;
            this.next = next;
            this.prev = prev;
        }

        private Node(Type data) {
            this(data, null, null);
        }
    }
}
```
## 5. 双链表示例结构
第一个节点的 prev 指向 null。

最后一个节点的 next 指向 null。

结构示意：

kotlin
Copy
null ← data ↔ data ↔ data ↔ data → null
head                                                        tail
## 6. 插入操作（Insertion）
例如，在节点 C 和 D 之间插入新节点 X：

新节点 X 的 prev 指向 C，next 指向 D。

更新 C 的 next 指向 X，更新 D 的 prev 指向 X。

结构示意：

csharp
Copy
null ← A ↔ B ↔ C ↔ D → null

插入 X 后：

null ← A ↔ B ↔ C ↔ X ↔ D → null
head                                         tail
7. 删除操作（Deletion）
删除节点 D（位于 C 和 E 之间）：

更新 C 的 next 指向 E。

更新 E 的 prev 指向 C。

节点 D 会被垃圾回收。

结构示意：

mathematica
Copy
null ← A ↔ B ↔ C ↔ D ↔ E → null

删除 D 后：

null ← A ↔ B ↔ C ↔ E → null
head                                tail
8. 链表总结及复杂度
链表是动态数据结构，可以存储基本类型或对象。

访问或搜索链表元素时间复杂度为 O(n)。

在头部添加或删除元素（有头指针）时间复杂度为 O(1)。

链表适合频繁插入、删除操作。

应用举例：

单链表：RFID 扫描、应用向导、游戏关卡顺序。

双链表：浏览器历史、滚动条、前进/后退按钮。

9. 循环链表（Circular Linked Lists）
循环链表的最后一个节点指向第一个节点，第一个节点也可指向最后一个节点。

可为单链表或双链表形式。

插入和删除操作与对应链表类似。

典型应用包括：

GIF 动画

音乐播放列表

操作系统的轮转调度算法（Round-robin）

示意：

单循环链表示例
bash
Copy
head → ... → tail
 ↑                 ↓
 └─────────────────┘
双循环链表示例
bash
Copy
head ↔ ... ↔ tail
 ↑                 ↓
 └─────────────────┘
