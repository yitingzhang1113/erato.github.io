---
layout: post
title: "数据结构之 Linked Lists（链表）详解"
date: 2025-06-03
tags: [数据结构, Linked Lists]
comments: true
author: Erato
---

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

element | next ---> element | next ---> ... ---> null
head

yaml
Copy

- 最后一个节点的 `next` 指向 `null`，表示链表结束。

---

## 4. 在头部插入节点（Insert at Head）

步骤：

1. 创建新节点，赋值数据。
2. 新节点的 `next` 指向旧头节点。
3. 更新头指针指向新节点。

示意：

原链表：A -> B -> C -> null
插入 X 后：
X -> A -> B -> C -> null
head

yaml
Copy

---

## 5. 在尾部插入节点（Insert at Tail）

步骤：

1. 创建新节点，赋值数据。
2. 新节点的 `next` 指向 `null`。
3. 让尾节点的 `next` 指向新节点。
4. 如果有尾指针，更新尾指针指向新节点。

示意：

原链表：A -> B -> C -> null
插入 X 后：
A -> B -> C -> X -> null
head tail


---

## 6. 删除头节点（Remove at Head）

步骤：

1. 将头指针指向当前头节点的下一个节点。
2. 旧头节点等待垃圾回收。

示意：

原链表：A -> B -> C -> null
删除头节点后：
B -> C -> null
head



---

## 7. 删除尾节点（Remove at Tail）

- 单链表中删除尾节点不高效。
- 需要遍历找到倒数第二个节点。
- 更新尾指针（如果存在）指向倒数第二个节点。
- 将倒数第二个节点的 `next` 置为 `null`。

---

## 8. 在任意位置插入或删除节点

- 可以在链表的任意位置插入或删除节点。
- 需更新相关节点的 `next` 指针（以及双链表中的 `prev` 指针）以保证链表结构完整且可遍历。

---

## 9. 链表操作伪代码示例

```pseudo
procedure AddFirst(e)
  node ← new Node(e)
  node.next ← head
  head ← node
  if list was empty then
    tail ← node
  end if
  size ← size + 1
end procedure

procedure AddLast(e)
  node ← new Node(e)
  node.next ← null
  if list was empty then
    head ← node
  else
    tail.next ← node
  end if
  tail ← node
  size ← size + 1
end procedure

procedure RemoveFirst()
  node ← head
  head ← head.next
  size ← size - 1
  return node
end procedure
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
# LeetCode 例题

代码随想录讲义：[链表基础](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E9%93%BE%E8%A1%A8%E7%9A%84%E7%B1%BB%E5%9E%8B)

## 232. 用栈实现队列

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
