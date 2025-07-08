---
layout: post
title: "数据结构之 Linked Lists详解"
date: 2025-06-03
tags: [data structure, Linked Lists]
comments: true
author: Erato
permalink: /create-blog-with-github-pages/2025-06-03/
---

代码随想录讲义：[链表基础](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E9%93%BE%E8%A1%A8%E7%9A%84%E7%B1%BB%E5%9E%8B)


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
假设链表为：

```css
head ──► A ──► B ──► C ──► null
                         ▲
                         │
                 C 是当前尾节点
```
目标：把 C 删掉，让 B 成为新尾节点。
❶ 空链表直接返回
size == 0 时表示 head.next == null，没有真正节点可删。

❷ prev 初值是 head
我们要找的是“倒数第二个”节点，所以用一个指针 prev 从虚拟头开始向后走。

❸ while 循环判定条件
prev.next != null ：确保 prev 不是最后一个（至少还有一个真节点）。

prev.next.next != null ：确保 prev.next 不是最后一个（至少还能再往后走）。
当这个条件不满足时，prev.next 就是 尾节点，而 prev 就停在了 倒数第二个。

走完循环后，示意图变成：

```css
head ──► A ──► B ──► C ──► null
                ▲
                │
              prev   （B）

```
❹ prev.next = null
断开 B 指向 C 的那条引用。

C 失去链表入口，等待 Java GC。

B 成为新的尾节点。
```css
head ──► A ──► B ──► null
```
❺ 更新 size
长度减 1，逻辑一致。

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



除了基本的增删查外的，其他变化，参考leetcode例题：
# 232. 移除链表元素
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
# 206.[反转链表](https://leetcode.cn/problems/reverse-linked-list/description/)
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



# 24. [两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)
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


# 19. [删除链表的倒数第N个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)
![删除链表的倒数第N个节点](/images/linkedlist6.png)
为什么要用「快慢指针」？
在单链表里我们无法 从尾部往前走，如果只知道要删倒数第 n 个节点，最朴素的办法是：先遍历一遍得到链表长度 L。计算要删除的节点是第 L-n+1 个。

再遍历第二遍走到它的前驱节点并修改指针。缺点：两次遍历，时间复杂度虽然仍是 O(L)，但扫描了两遍链表；而且要多存一个长度变量。

快慢指针的一遍做法
快慢指针（two-pointer / runner technique）的核心思想是 让两根指针保持固定间距前进。具体到删除倒数第 n 个节点：

1.准备一个虚拟头结点 dummy

这样即使删除的是原来头结点，dummy.next 仍指向新的头结点，避免大量特判。

2.让 fast 先走 n+1 步

步数 = n（两节点间距） + 1（因为想让 slow 最终停在待删节点的 前驱）。

这一步建立了：fast 与 slow 始终 相差 n+1 个节点 的距离。

3.快慢指针同时前进

当 fast 指向 null（越过尾端）时，slow 正好在待删除节点的前驱上。

只需一次遍历就定位目标。

4.修改指针完成删除

slow.next = slow.next.next;

java版本
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        //新建一个虚拟头节点指向head
        ListNode dummyNode = new ListNode(0);
        dummyNode.next = head;
        //快慢指针指向虚拟头节点
        ListNode fastIndex = dummyNode;
        ListNode slowIndex = dummyNode;

        // 只要快慢指针相差 n 个结点即可
        for (int i = 0; i <= n; i++) {
            fastIndex = fastIndex.next;
        }
        //同时走，走到快指针尽头
        while (fastIndex != null) {
            fastIndex = fastIndex.next;
            slowIndex = slowIndex.next;
        }

        // 此时 slowIndex 的位置就是待删除元素的前一个位置。
        // 具体情况可自己画一个链表长度为 3 的图来模拟代码来理解
        // 检查 slowIndex.next 是否为 null，以避免空指针异常
        if (slowIndex.next != null) {
            slowIndex.next = slowIndex.next.next;
        }
        return dummyNode.next;
    }
}

```

时间复杂度	O(L)	只遍历链表一次（快指针先走 n + 1 步，再与慢指针同步前进），其中 L 为链表长度
空间复杂度	O(1)	仅使用常数级额外变量：dummy、fast、slow 三个指针

# 双链表（Doubly Linked List）

- 每个节点含有两个指针：一个指向下一个节点，一个指向前一个节点。
- 可双向遍历链表（向前与向后）。
- 节点包含：
  - 元素（element）
  - 指向前节点的指针（prev）
  - 指向后节点的指针（next）

结构示意：
![Doubly Linked List](/images/linkedlist2.png)


---

## 1. 双链表节点的泛型代码示例（Java）

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
 
## 2.双链表的增删查
                                                    
### 1. 插入操作（Insertion）
![Doubly Linked List Insertion](/images/linkedlist3.png)
### 2. 删除操作（Deletion）
![Doubly Linked List deletion](/images/linkedlist4.png)  


```java
/**
 * 双向链表：支持在头/尾/任意索引插入与删除，并能按索引获取元素。
 * 仅演示 int 数据，如需泛型可改为 <T>。
 */
public class DoublyLinkedList {

    /** 内部节点定义 */
    private static class Node {
        int val;
        Node prev, next;
        Node(int val) { this.val = val; }
    }

    private int size;      // 链表元素个数
    private final Node head; // 虚拟头结点 (sentinel)
    private final Node tail; // 虚拟尾结点 (sentinel)

    /** 初始化：首尾使用哨兵节点，避免大量空指针判断 */
    public DoublyLinkedList() {
        head = new Node(0);
        tail = new Node(0);
        head.next = tail;
        tail.prev = head;
        size = 0;
    }

    /* ========= 查 (Get) ========= */
    /** O( min(index, size-index) ) 获取指定索引的值；越界返回 -1 */
    public int get(int index) {
        Node target = nodeAt(index);
        return target == null ? -1 : target.val;
    }

    /* ========= 增 (Insertion) ========= */
    /** 头插：O(1) */
    public void addAtHead(int val) { insertBetween(new Node(val), head, head.next); }

    /** 尾插：O(1) */
    public void addAtTail(int val) { insertBetween(new Node(val), tail.prev, tail); }

    /** 任意索引插入：索引为 size 相当于尾插；非法索引忽略 */
    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size) return;
        // 找到插入位置的前驱节点
        Node prev = (index <= size / 2) ? head : tail;
        int steps = (index <= size / 2) ? index : size - index;
        for (int i = 0; i < steps; i++)
            prev = (index <= size / 2) ? prev.next : prev.prev;
        Node next = prev.next;
        insertBetween(new Node(val), prev, next);
    }

    /* ========= 删 (Deletion) ========= */
    /** 删首：O(1) */
    public void removeAtHead() { deleteNode(head.next); }

    /** 删尾：O(1) */
    public void removeAtTail() { deleteNode(tail.prev); }

    /** 任意索引删除：非法索引忽略 */
    public void deleteAtIndex(int index) {
        Node target = nodeAt(index);
        if (target != null) deleteNode(target);
    }

    /* ========= 私有辅助 ========= */
    /** 将 newNode 插入到 prev 与 next 之间 */
    private void insertBetween(Node newNode, Node prev, Node next) {
        newNode.prev = prev;
        newNode.next = next;
        prev.next = newNode;
        next.prev = newNode;
        size++;
    }

    /** 删除单个节点（哨兵节点除外） */
    private void deleteNode(Node node) {
        if (node == head || node == tail) return; // 忽略哨兵
        node.prev.next = node.next;
        node.next.prev = node.prev;
        size--;
    }

    /** 返回索引处节点；越界返回 null （不含哨兵） */
    private Node nodeAt(int index) {
        if (index < 0 || index >= size) return null;
        Node cur;
        if (index <= size / 2) {            // 从头走更近
            cur = head.next;
            for (int i = 0; i < index; i++) cur = cur.next;
        } else {                            // 从尾走更近
            cur = tail.prev;
            for (int i = size - 1; i > index; i--) cur = cur.prev;
        }
        return cur;
    }

    /** 当前链表元素个数 */
    public int size() { return size; }

    /* ========== 简单测试 ========== */
    public static void main(String[] args) {
        DoublyLinkedList list = new DoublyLinkedList();
        list.addAtHead(1); // 1
        list.addAtTail(3); // 1,3
        list.addAtIndex(1, 2); // 1,2,3
        System.out.println(list.get(1)); // 2
        list.deleteAtIndex(1); // 1,3
        System.out.println(list.get(1)); // 3
        list.removeAtHead(); // 3
        list.removeAtTail(); // 空
        System.out.println(list.size()); // 0
    }
}
```


707[设计链表](https://leetcode.cn/problems/design-linked-list/description/)

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


# 链表总结及复杂度
链表是动态数据结构，可以存储基本类型或对象。

访问或搜索链表元素时间复杂度为 O(n)。

在头部添加或删除元素（有头指针）时间复杂度为 O(1)。

链表适合频繁插入、删除操作。

应用举例：

单链表：RFID 扫描、应用向导、游戏关卡顺序。

双链表：浏览器历史、滚动条、前进/后退按钮。
# 循环链表（Circular Linked Lists）
循环链表的最后一个节点指向第一个节点，第一个节点也可指向最后一个节点。
可为单链表或双链表形式。插入和删除操作与对应链表类似。

典型应用包括：GIF 动画,音乐播放列表,操作系统的轮转调度算法（Round-robin）

示意：

单循环链表示例
![Single Circular Linked List Example](/images/linkedlist5.png)

```java
/**
 * 单向循环链表（只存 int，若需泛型可改为 <T>）
 */
public class CircularLinkedList {

    /* ============ 节点定义 ============ */
    private static class Node {
        int val;
        Node next;
        Node(int val) { this.val = val; }
    }

    private Node tail;  // 指向最后一个节点，tail.next == head
    private int size;   // 链表长度

    /* ============ 构造 ============ */
    public CircularLinkedList() {
        tail = null; // 空链表
        size = 0;
    }

    /* ============ 查 (Get) ============ */
    /** 获取索引处元素；越界返回 -1 */
    public int get(int index) {
        Node node = nodeAt(index);
        return node == null ? -1 : node.val;
    }

    /* ============ 增 (Insertion) ============ */
    /** 头插：O(1) */
    public void addAtHead(int val) {
        Node newNode = new Node(val);
        if (tail == null) {          // 空链表
            newNode.next = newNode;  // 自环
            tail = newNode;
        } else {
            newNode.next = tail.next; // 指向原 head
            tail.next = newNode;      // tail.next 更新为新 head
        }
        size++;
    }

    /** 尾插：O(1) */
    public void addAtTail(int val) {
        addAtHead(val);    // 先复用头插逻辑
        tail = tail.next;  // 再把 tail 向后移动一格
    }

    /** 任意索引插入：index==size 即尾插；非法索引忽略 */
    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size) return;
        if (index == 0) { addAtHead(val); return; }
        if (index == size) { addAtTail(val); return; }

        Node prev = nodeAt(index - 1);
        Node newNode = new Node(val);
        newNode.next = prev.next;
        prev.next = newNode;
        size++;
    }

    /* ============ 删 (Deletion) ============ */
    /** 删头：O(1) */
    public void removeAtHead() {
        if (size == 0) return;
        if (size == 1) {           // 仅一个节点
            tail = null;
        } else {
            tail.next = tail.next.next; // 跳过原 head
        }
        size--;
    }

    /** 删尾：O(1) */
    public void removeAtTail() {
        if (size == 0) return;
        if (size == 1) {
            tail = null;
        } else {
            // 找倒数第二个节点
            Node prev = tail.next;
            while (prev.next != tail) prev = prev.next;
            prev.next = tail.next; // 断开尾
            tail = prev;           // 更新 tail
        }
        size--;
    }

    /** 任意索引删除：非法索引忽略 */
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) return;
        if (index == 0) { removeAtHead(); return; }
        if (index == size - 1) { removeAtTail(); return; }

        Node prev = nodeAt(index - 1);
        prev.next = prev.next.next; // 跳过目标节点
        size--;
    }

    /* ============ 工具 ============ */
    /** 返回索引处节点；越界返回 null */
    private Node nodeAt(int index) {
        if (index < 0 || index >= size) return null;
        Node cur = tail.next; // head
        for (int i = 0; i < index; i++) cur = cur.next;
        return cur;
    }

    /** 当前长度 */
    public int size() { return size; }

    /* ============ 简单测试 ============ */
    public static void main(String[] args) {
        CircularLinkedList clist = new CircularLinkedList();
        clist.addAtHead(1);         // 1
        clist.addAtTail(3);         // 1,3
        clist.addAtIndex(1, 2);     // 1,2,3
        System.out.println(clist.get(1)); // 2
        clist.deleteAtIndex(1);     // 1,3
        System.out.println(clist.get(1)); // 3
        clist.removeAtHead();       // 3
        clist.removeAtTail();       // 空
        System.out.println(clist.size()); // 0
    }
}
```

leetcode 例题

[160.链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)

双指针“拼接”法（经典写法）

![双指针拼接](/images/linkedlist7.png)

核心思想
让两指针走过相同的“路径长度”：

p1 先遍历 A，到尾部后跳到 B；

p2 先遍历 B，到尾部后跳到 A。

如果链表相交：

当两条路径长度相等时，两指针恰好在交点相遇。

如果链表不相交：

两指针最终同时变为 null，循环退出后返回 null。

等价于“补齐长度”
指针在第一次到尾部后“换跑道”，补上另一条链表的长度差；因此总路程都是 m + n，相交则在第 m + n − k 步（k 为交点之后的尾部长度）相遇。

复杂度

指标	复杂度
时间	O(m + n) ：每个指针最多走两次全长
空间	O(1)

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
		// p1 指向 A 链表头结点，p2 指向 B 链表头结点
		ListNode p1 = headA, p2 = headB;
		while (p1 != p2) {
			// p1 走一步，如果走到 A 链表末尾，转到 B 链表
			if (p1 == null) p1 = headB;
			else            p1 = p1.next;
			// p2 走一步，如果走到 B 链表末尾，转到 A 链表
			if (p2 == null) p2 = headA;
			else            p2 = p2.next;
		}
		return p1;
    }
}
```


[142. Linked List Cycle II](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

题意： 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。
这道题目，不仅考察对链表的操作，而且还需要一些数学运算。

主要考察两知识点：

判断链表是否环----可以使用快慢指针法，分别定义 fast 和 slow 指针，从头结点出发，fast指针每次移动两个节点，slow指针每次移动一个节点，如果 fast 和 slow指针在途中相遇 ，说明这个链表有环。

如果有环，如何找到这个环的入口
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {// 有环
                ListNode index1 = fast;
                ListNode index2 = head;
                // 两个指针，从头结点和相遇结点，各走一步，直到相遇，相遇点即为环入口
                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        return null;
    }
}
```





