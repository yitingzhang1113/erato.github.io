---
layout: post
title: "数据结构之 Stack（堆栈）与队列（Queue）详解"
date: 2025-06-02
tags: [Stack（堆栈）与队列（Queue）]
comments: true
author: Erato
permalink: /create-blog-with-github-pages/2025-06-02/
---
队列是先进先出，栈是先进后出。
![stack and queue](/images/stack.png)
# Stack（堆栈）
# 一、什么是 Stack（堆栈）

Stack（堆栈）是一种特殊的数据结构，遵循“后进先出”（LIFO, Last-In First-Out）原则。  
这意味着，最后被放入堆栈的元素，将是第一个被取出的元素。  

- 栈只允许在顶端（Top）进行元素的插入（push）和删除（pop）操作。  
- 你无法直接访问、插入或删除栈中间或底部的元素，只有栈顶元素可被操作。

它本质上是一种抽象数据类型（ADT），并没有唯一的具体实现方式。栈提供push 和 pop 等等接口，所有元素必须符合先进后出规则，所以栈不提供走访功能，也不提供迭代器(iterator)。 不像是set 或者map 提供迭代器iterator来遍历所有元素。栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的（也就是说我们可以控制使用哪种容器来实现栈的功能）。
![stack](/images/stack2.png)
### Java 中的“栈”与底层容器适配（简述版）

- **思想**  
  - *栈* 只是暴露 `push / pop / peek` 接口，真正存数据的活交给 **可插拔容器**。  
  - 这与 C++ STL 的 *container adapter* 概念一致。

- **推荐接口**：`Deque<E>`  
  - `ArrayDeque` → 动态环形数组  
  - `LinkedList` → 双向链表  
  - `ConcurrentLinkedDeque` → 无锁链表（并发场景）

- **把 Deque 当栈用**  
**Deque**（读作 *“deck”*，全称 *Double-Ended Queue*）是一种**双端队列**数据结构——  
- **双端**：既可以在队头也可以在队尾插入或删除元素。  
- **队列**：保持元素的顺序，但不像普通队列只开放一端，而是两端都可操作。

  - `push(x)` ⇔ `deque.addFirst(x)`  
  - `pop()` ⇔ `deque.removeFirst()`  
  - `peek()` ⇔ `deque.peekFirst()`

- **示例：换底座只改一行**
  ```java
  Deque<Integer> stack = new ArrayDeque<>();     // 数组实现
  // Deque<Integer> stack = new LinkedList<>();  // 换成链表实现
  stack.push(42);
  int top = stack.pop();
队列同理
Queue<E> / Deque<E> + 任意实现 → FIFO 或双端队列。
![stack3](/images/stack3.png)
---

# 二、Stack 的基本操作
### 主要操作
- `push(object)`：将元素 `object` 压入栈顶  
- `pop()`：移除并返回栈顶元素  

### 辅助操作
- `top()` / `peek()`：返回栈顶元素，但不删除  
- `size()`：返回当前栈元素个数  
- `isEmpty()`：判断栈是否为空  

| 操作        | 描述                                | 返回值                         |
|-------------|-------------------------------------|--------------------------------|
| `push(object)` | 将元素 `object` 插入栈顶               | 无                             |
| `pop()`     | 移除并返回栈顶元素                   | 栈顶元素（若栈空，则返回 `null`）|
| `top()` / `peek()` | 返回栈顶元素，但不删除它         | 栈顶元素（若栈空，则返回 `null`）|
| `size()`    | 返回栈中当前元素个数                 | 整数                           |
| `isEmpty()` | 判断栈是否为空                      | 布尔值（空返回 `true`，否则 `false`）|

---

# 三、Stack 的示例操作流程

假设栈初始为空，按顺序执行以下操作：

| 操作      | 返回值 | 栈状态（从栈顶到栈底）     |
|-----------|---------|----------------------------|
| `push(5)` | -       | 5                          |
| `push(3)` | -       | 3, 5                       |
| `size()`  | 2       | 3, 5                       |
| `pop()`   | 3       | 5                          |
| `isEmpty()` | false | 5                          |
| `pop()`   | 5       | （）                     |
| `isEmpty()` | true  | （）                     |
| `pop()`   | null    | （）                     |
| `push(7)` | -       | 7                          |
| `push(9)` | -       | 9, 7                       |
| `top()`   | 9       | 9, 7                       |
| `push(4)` | -       | 4, 9, 7                    |
| `size()`  | 3       | 4, 9, 7                    |

---

# 四、Stack 的实现方式

作为抽象数据类型，Stack 可以通过不同底层结构实现，常见的有：

### 1. 数组实现（Array-backed Stack）

- 使用数组存储元素，按照从左到右顺序放置，栈顶位于数组末尾。  
- 维护一个变量表示栈顶元素的索引或当前栈大小。  
- 插入时将元素放到数组 `size` 索引处，`size` 加一；删除时将 `size` 减一并返回对应元素。  
- 数组容量有限，当满时需要扩容或抛出异常。扩容操作为 O(n) 时间，但摊销到每次 `push` 为 O(1)。
The array storing the stack elements may become full. A
push() operation may then throw an implementation-defined
exception or resize the backing array. This behavior is not
defined or forced by the ADT.  

**示例伪代码：**

```pseudo
procedure push(obj)
  arr[size] ← obj
  size ← size + 1
end procedure

procedure pop
  if size == 0 then return null
  size ← size - 1
  item ← arr[size]
  arr[size] ← null
  return item
end procedure
```

## 2. 链表实现（Linked List-backed Stack）

- 使用链表节点存储元素，栈顶对应链表头。
- `push` 和 `pop` 操作均在链表头部进行，时间复杂度 O(1)。
- 不需要预定义容量，动态增长，无扩容问题。

**示例示意：**  
`head -> 4 -> 9 -> 7 -> null`，`head` 指向栈顶元素 4。

---

## 五、性能分析与限制


### 1. 复杂度概览
- 设栈中元素数量为 *n*  
- **空间**：`O(n)`  
- **时间**：`push(obj)` | `pop()` | `top()` | `size()` | `isEmpty()` → `O(1)`  

### 2. 数组栈 vs. 链表栈

| 指标 / Metric            | 数组实现 Array-Backed Stack              | 链表实现 Linked-List Stack         |
|--------------------------|------------------------------------------|------------------------------------|
| **空间复杂度** Space     | `O(n)`；容量固定，超出需扩容             | `O(n)`；动态增长                   |
| **push 时间** Push Time  | 平均 `O(1)`；<br>触发扩容时 `O(n)`       | 始终 `O(1)`                        |
| **pop 时间** Pop Time    | `O(1)`                                   | `O(1)`                             |
| **top 时间** Top/Peek    | `O(1)`                                   | `O(1)`                             |
| **主要限制** Limitations | 需预设容量；扩容代价高                   | 需额外指针字段，略增空间开销        |

### 3. 关键限制 (Key Limitations)
1. **数组栈**  
   - 必须预定义最大容量；一旦满需整体扩容，单次扩容成本 `O(n)`  
   - 尽管如此，摊销后 `push()` 仍视作 `O(1)`  
2. **链表栈**  
   - 无需预定义容量，天然支持动态增长  
   - 但每个节点存储指针，内存占用稍高

---

## 六、Stack 的应用

### 直接应用

- 浏览器历史记录：记录页面访问顺序，实现“后退”功能。  
- 文本编辑器撤销操作：保存操作记录，实现撤销（Undo）功能。  
- 程序调用栈（Activation Stack）：保存函数调用信息，包括局部变量、返回地址，支持递归调用。  

### 间接应用

- 作为算法的辅助数据结构，如括号匹配、表达式求值、深度优先搜索（DFS）等。  
- 组成其他复杂数据结构的基础组件。  

---

## 七、函数调用栈详解（Activation Stack）

- 现代编程语言（如 Java）使用栈来管理函数调用。  
- 调用函数时，创建一个“激活记录（activation frame）”，压入栈顶，包含：  
  - 函数的局部变量和返回值存储  
  - 程序计数器（记录当前执行的语句位置）  
- 函数执行完毕后，激活记录从栈顶弹出，程序控制权返回上一个函数。  
- 该机制使递归调用得以正确执行。  

---

## 八、括号匹配示例
题目链接：[20. Valid Parentheses](https://leetcode.cn/problems/valid-parentheses/description/)
![stack](/images/stack4.png)
```Pseudo
函数 isValid（字符串 s）返回 布尔
    创建空栈 stack          # 用来保存“期望出现的右括号”

    对 s 的每个字符 ch 做
        如果 ch 是左括号                   # ‘(’  ‘{’  ‘[’
            根据 ch 对应的右括号 r
            将 r 压入 stack               # 说明我们接下来“期待”遇到 r
            跳到下一个字符
        否则                              # ch 是右括号
            如果 stack 为空               # 没有任何等待配对的右括号
                返回 假                   # 提前判定失败
            如果 stack.top() ≠ ch         # 等待的括号类型与 ch 不匹配
                返回 假
            弹出 stack.top()              # 成功配对，栈顶出栈
    循环结束

    如果 stack 为空                       # 没有遗留的等待配对项
        返回 真                           # 全部匹配成功
    否则
        返回 假                           # 还有多余的左括号
结束函数

```
```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> deque = new LinkedList<>();
        char ch;
        for (int i = 0; i < s.length(); i++) {
            ch = s.charAt(i);
            //碰到左括号，就把相应的右括号入栈
            if (ch == '(') {
                deque.push(')');
            }else if (ch == '{') {
                deque.push('}');
            }else if (ch == '[') {
                deque.push(']');
            } else if (deque.isEmpty() || deque.peek() != ch) {
                return false;
            }else {//如果是右括号判断是否和栈顶元素匹配
                deque.pop();
            }
        }
        //遍历结束，如果栈为空，则括号全部匹配
        return deque.isEmpty();
    }
}

```

---

## 九、总结

堆栈（Stack）是计算机科学中极为重要且基础的数据结构，遵循后进先出原则，简单而高效。  
掌握其实现、操作和应用，有助于深入理解程序执行机制及各种算法设计。
# 队列（Queues）

## 1. 队列简介
队列是一种数据结构，遵循**先进先出（FIFO，First-In First-Out）**原则。  
即：最先加入队列的元素最先被移除。

- 元素的插入发生在队列的尾部（rear），  
- 元素的删除发生在队列的头部（front）。  
- 不允许在队列的中间或其他位置进行添加、删除或访问。

队列是抽象数据类型（ADT），有多种实现方式。

---

## 2. 队列的主要操作

| 操作            | 说明                       |
| --------------- | -------------------------- |
| `enqueue(object)` | 在队列尾部插入一个元素      |
| `dequeue()`      | 移除并返回队列头部的元素    |
| `first()`        | 返回队列头部元素但不移除    |
| `size()`         | 返回队列中元素的数量        |
| `isEmpty()`      | 判断队列是否为空，若无元素则返回 `true` |

---

## 3. 操作示例

| 操作          | 返回值 | 队列状态         |
| ------------- | ------ | ---------------- |
| `enqueue(5)`  | -      | (5)              |
| `enqueue(3)`  | -      | (5, 3)           |
| `dequeue()`   | 5      | (3)              |
| `enqueue(7)`  | -      | (3, 7)           |
| `dequeue()`   | 3      | (7)              |
| `first()`     | 7      | (7)              |
| `dequeue()`   | 7      | ()               |
| `dequeue()`   | null   | ()               |
| `isEmpty()`   | true   | ()               |
| `enqueue(9)`  | -      | (9)              |
| `enqueue(7)`  | -      | (9, 7)           |
| `size()`      | 2      | (9, 7)           |
| `enqueue(3)`  | -      | (9, 7, 3)        |
| `enqueue(5)`  | -      | (9, 7, 3, 5)     |
| `dequeue()`   | 9      | (7, 3, 5)        |

---

## 4. 基于数组的队列实现（Array-backed Queue）

使用大小为 N 的数组模拟循环队列（环形队列）。

维护两个变量：

- `front`：队列头元素的索引  
- `size`：队列中元素的数量  

队尾索引计算公式：

```pseudo
back = (front + size) mod N
表示数组中第一个空槽。

procedure enqueue(o)
  back ← (front + size) mod N
  arr[back] ← o
  size ← size + 1
end procedure

procedure dequeue
  item ← arr[front]
  front ← (front + 1) mod N
  size ← size − 1
  return item
end procedure
```
注意：出队后，不必将出队位置清空（置空）。
需要注意当队列为空时不能进行出队操作。

## 5. 基于链表的队列实现（Linked List-backed Queue）
使用链表作为队列的底层数据结构。

元素插入和删除分别发生在链表的两端（具体端点视链表实现而定）。

实现灵活，避免了数组容量限制和环形处理。

## 6. 性能分析
设队列元素个数为 n。

空间复杂度：O(n)

主要操作（入队、出队、查看首元素等）时间复杂度均为：O(1)

## 7. 队列的局限性
数组实现需要预先定义最大容量 N。

如果要支持动态扩容，扩容过程为 O(n)，不过单次 enqueue() 的摊销时间复杂度仍然是 O(1)。

扩容时队列元素会被“展开”至新数组的开头。

链表实现不受上述限制。

## 8. 队列的应用
直接应用

等候队列（Waiting lists）

共享资源访问控制（如打印机队列）

多线程任务调度

间接应用

算法辅助数据结构

其他复杂数据结构的组成部分

## 9. 案例：轮转调度（Round Robin Scheduler）
多进程时，CPU 按轮转方式分配时间片。

使用队列维护所有待执行进程。

轮转调度实现步骤：

```pseudo
process = processes.dequeue()   # 取出队首进程
对该进程执行工作
processes.enqueue(process)      # 将进程重新加入队尾
```
# LeetCode 例题

代码随想录讲义：[树与队列理论基础](https://programmercarl.com/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## 232. 用栈实现队列

题目链接：[LeetCode 232. Implement Queue using Stacks](https://leetcode.cn/problems/implement-queue-using-stacks/)


## 题目概述

设计一个队列，使其只能使用两个栈实现以下操作：

- `push(x)`：将元素 `x` 推入队列尾部。
- `pop()`：移除并返回队列头部元素。
- `peek()`：返回队列头部元素。
- `empty()`：判断队列是否为空。

队列是先进先出（FIFO）结构，而栈是后进先出（LIFO）结构。如何用两个后进先出的栈来实现先进先出的队列，是本题的核心。

---

## 关键难点

- **单个栈无法实现队列的先进先出特性。**
- 需要用两个栈，一个用于输入（push操作），一个用于输出（pop和peek操作）。
- 如何实现两个栈协同完成队列操作，保证顺序正确。

---

## 思路解析

1. **两个栈的设计**

   - **输入栈（stackIn）**：用于执行 `push` 操作，所有新元素都压入这个栈。
   - **输出栈（stackOut）**：用于执行 `pop` 和 `peek` 操作，从这个栈弹出或查看元素。

2. **为什么两个栈可以模拟队列？**

   - 栈本身是后进先出。
   - 当需要从队列头部取元素时，如果输出栈为空，则将输入栈的所有元素依次弹出并压入输出栈。
   - 这样输出栈的栈顶元素就是队列的头部元素，实现了先进先出的效果。

3. **操作流程**

   - **push(x)**：直接将元素压入输入栈。
   - **pop()**：
     - 若输出栈为空，将输入栈所有元素倒入输出栈。
     - 从输出栈弹出栈顶元素，即队头元素。
   - **peek()**：
     - 同 `pop()` 逻辑确保输出栈不空。
     - 返回输出栈栈顶元素，但不弹出。
   - **empty()**：当输入栈和输出栈都为空时，队列为空。

4. **复杂度分析**

   - **时间复杂度**：
     - 每个元素最多被压入和弹出两次（分别进出两个栈）。
     - 摊销时间复杂度为 O(1)。
   - **空间复杂度**：
     - 需要两个栈的空间，空间复杂度为 O(n)。

5. **代码复用和设计建议**

   - `peek()` 与 `pop()` 操作类似，可以复用代码，避免重复。
   - 编写辅助函数 `dumpStackInToOutIfNeeded()` 来管理两个栈间元素转移，提高代码清晰度。
   - 生产环境代码应避免复制粘贴，提倡复用和抽象。

---

## 总结

- 本题核心是用两个栈倒置元素顺序，模拟队列先进先出。
- 输入栈负责接收所有新元素，输出栈负责弹出和查看队首元素。
- 通过辅助函数实现栈间元素倒换，保证操作正确。
- 摊销时间复杂度为 O(1)，实现高效的队列操作。

---

## 参考伪代码

```pseudo
class MyQueue {
    stackIn
    stackOut

    method push(x):
        stackIn.push(x)

    method pop():
        dumpStackInToOutIfNeeded()
        return stackOut.pop()

    method peek():
        dumpStackInToOutIfNeeded()
        return stackOut.peek()

    method empty():
        return stackIn.isEmpty() AND stackOut.isEmpty()

    private method dumpStackInToOutIfNeeded():
        if stackOut is not empty:
            return
        while stackIn is not empty:
            stackOut.push(stackIn.pop())
}

