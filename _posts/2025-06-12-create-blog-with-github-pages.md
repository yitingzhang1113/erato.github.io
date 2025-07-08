---
layout: post
title: "Heap（堆）"
date: 2025-06-12
tags: [data structure,堆]
comments: true
author: Erato
mathjax: true
permalink: /create-blog-with-github-pages/2025-06-12/
---

# Heap 简介 | Heap Definition  
堆是一种**完全二叉树**（Complete Binary Tree）结构，用来快速取得集合中的最大值或最小值。  
*A heap is a **complete binary tree** that lets us retrieve the largest or smallest element in **O(1)** time.*

---

## 为什么使用 Heap？ | Why Do We Need a Heap?  
- **快速访问极值**：只要查看根节点即可。  
  *Fast extreme lookup: just read the root node.*  
- **插入 / 删除堆顶仅 `O(log n)`**，比线性表的最坏 `O(n)` 高效。  
  *Insert / delete-top takes only `O(log n)`, versus `O(n)` in a list.*  

---

## Heap 类型 | Heap Types  

| 中文 | English | 描述 / Description |
| ---- | ------- | ------------------ |
| **最大堆** | **Max-Heap** | 根节点是最大值 (root holds the largest element) |
| **最小堆** | **Min-Heap** | 根节点是最小值 (root holds the smallest element) |

---
##  Properties of a Heap / 堆的两大性质
### • Order Property / **有序性**
- In a *max-heap* every parent is **≥** its children; in a *min-heap* every parent is **≤** its children.  
- 在 **最大堆** 中，父节点的键值 **≥** 任一子节点；在 **最小堆** 中，父节点的键值 **≤** 任一子节点。  
> Children are not ordered relative to each other.  
> 子节点之间彼此没有顺序要求。  

### • Shape Property / **结构完整性**
- A heap is always a **complete binary tree**—all levels are full except possibly the last, which is filled from **left to right** without gaps.  
- 堆始终保持 **完全二叉树** 形态——除最底层外，其余层节点全部填满；最底层从 **左到右** 连续填充，不允许空缺。  

> ⚠ Only the root (the global max or min) is directly accessible; arbitrary search is *not* supported.  
> ⚠ 只有根节点（全局最大或最小）可以直接访问，堆不支持任意位置搜索。  

---

## Binary Heap as an Array / 数组式二叉堆
Thanks to the two properties, we can map a heap to an array very elegantly.    
借助上述性质，堆可被优雅地映射到数组中：  


| Concept      | Formula                                    | 说明 / Note                    |
|--------------|--------------------------------------------|--------------------------------|
| Root index   | 1 (skip 0)                                 | 为简化运算，索引 0 留空         |
| Left child   | $2i$                                       | $i$ 的左孩子                   |
| Right child  | $2i + 1$                                   | $i$ 的右孩子                   |
| Parent       | $i/2$  | $i$ 的父节点（整除）           |


> This representation gives **O(1)** access to parent/child indices without pointers.  
> 数组表示让父子节点定位变为 **O(1)**，无需指针。  

---

## Adding (an Insertion) / 插入操作

1. **Place**    
   - Insert the new key at the **next free slot** (end of array).  
   - 将待插入元素放在 **数组末尾** 的下一个空位。  

2. **Fix–Up (Heapify-Up)**    
   - Compare the new node with its parent.  
   - 如果违反有序性（max-heap 中 *parent < child*，或 min-heap 中 *parent > child*），交换两者。  
   - Recursively (or iteratively) repeat until either  
     1. **No swap** is needed, or  
     2. The node reaches the **root**.  
   - 该过程称为 **heapify-up**（上浮），时间复杂度 $$O(\log n)$$，因树高为 $$\log_2 n$$。  


![Heap insertion example](/images/images.gif)

```procedure
procedure Add(data)
    index ←next empty slot in the heap
    heap[index ] ←data
    parentIndex ←index /2
    while parentIndex >= 1, and heap[parentIndex ] and heap[index ] are not in the correct order do
        Swap heap[parentIndex ] and heap[index ]
        index ←parentIndex
        parentIndex ←parentIndex /2
    end while
end procedure
```
java version
```java
import java.util.Arrays;

import java.util.NoSuchElementException;

/**
 * 简易版最大堆（Max-Heap）。
 *
 * @param <T> 堆中元素必须可比较
 */
@SuppressWarnings("unchecked")
public class MaxHeap<T extends Comparable<? super T>> {

    private static final int INITIAL_CAPACITY = 13;   // 与 CS 课程常用设置一致
    private T[] backingArray;   // 1-based：索引 0 空着
    private int size;           // 已存元素个数

    /** 构造空堆 */
    public MaxHeap() {
        backingArray = (T[]) new Comparable[INITIAL_CAPACITY];
        size = 0;
    }

    /**
     * 向堆中插入一个元素（上滤 / sift-up）
     *
     * @param data 待插入数据，禁止 null
     * @throws IllegalArgumentException 若 data 为 null
     */
    public void add(T data) {
        if (data == null) {
            throw new IllegalArgumentException("Cannot insert null into heap.");
        }

        // 扩容（若已满：size + 1 == backingArray.length，因为 index 0 空闲）
        if (size + 1 == backingArray.length) {
            grow();
        }

        // 1. 把元素放到“下一个空槽位”——数组尾部
        int index = ++size;
        backingArray[index] = data;

        // 2. 上滤：与父节点比较，若不满足堆序则交换
        int parentIndex = index / 2;
        while (parentIndex >= 1 &&
               backingArray[parentIndex].compareTo(backingArray[index]) < 0) {
            swap(parentIndex, index);
            index = parentIndex;
            parentIndex = index / 2;
        }
    }
```
## removing (a Deletion) / 删除操作
### 堆删除操作概览  
> 以 **最大堆 (max-heap)** 为例，最小堆 (min-heap) 只需把“大 ↔ 小”互换。

| 步骤 | 英文关键字 | 中文说明 |
| :--- | :--------- | :------ |
| 1 | **Remove root** | **删除根节点**（堆顶） |
| 2 | **Move last to root** | **将最后一个元素挪到根**，暂时可能破坏堆序 |
| 3 | **Compare with children** | 与左右孩子比较键值 |
| 4 | **Swap with larger child** | 若某孩子更大（大根堆）则与“更大”的孩子交换 |
| 5 | **Repeat (sift-down)** | 重复“比较→交换”直至无需交换或到达叶子 |

:::info 复杂度  
- **时间**：`O(log n)`（最多沿树高交换 `⌈log₂ n⌉` 次）  
- **空间**：`O(1)`（原地操作，无额外存储）  
:::

![Heap removal example](/images/images2.gif)
---
```procedure
procedure Removing
    index ←last filled slot in the heap
    data ←heap[1]
    heap[1] ←heap[index ]
    heap[index ] ←NULL
    index ←1
    while heap[index ] has a child, and heap[index ] and its left
    and right children are not in the correct order do
        Swap the largest/smallest child with heap[index ]
        index ←index of the largest/smallest child
    end while
    return data
end procedure
```

### Java 代码示例

```java
/**
 * 删除并返回堆顶（最大堆示例）
 */
public T remove() {
    if (size == 0) {
        throw new NoSuchElementException("Heap is empty.");
    }
    T root = backingArray[1];            // 1. 保存根节点
    backingArray[1] = backingArray[size];
    backingArray[size--] = null;         // 2. 尾元素补到根 & 缩小 size
    heapify(1);                          // 3. 下滤（恢复堆序）
    return root;
}

/** 下滤 (sift-down) **/
private void heapify(int i) {
    while (i * 2 <= size) {
        int left = i * 2, right = left + 1;
        int larger = (right <= size &&
                      backingArray[right].compareTo(backingArray[left]) > 0)
                     ? right : left;

        if (backingArray[i].compareTo(backingArray[larger]) >= 0) break;

        swap(i, larger);
        i = larger;
    }
}
```

## 1 · Complexity Summary 复杂度总结

| Operation | **Time Complexity** | 说明 (EN) | 说明 (中) |
|-----------|--------------------|-----------|-----------|
| **Insert / Add** | Worst / Avg : `O(log n)`<br>Best : `O(1)` | Up-heap / sift-up may climb to the root (`⌈log₂ n⌉` swaps).<br>Best-case when elements are pre-sorted **ascending** for a **min-heap** (or descending for a **max-heap**): new node stays a leaf. | 上滤最多爬高 `⌈log₂ n⌉` 层。<br>若 **小根堆按升序**（或大根堆按降序）连续插入，插入元素永远位于叶子，无需交换，故最佳 `O(1)`。 |
| **Remove / Extract-Root** | `O(log n)` | Last element moves to root, then down-heap / heapify travels ≤ `log₂ n` levels. | 尾元素补位后下滤，最多下降 `log₂ n` 层。 |
| **Space** | `O(n)` | Array of length `n + 1` (index 0 unused if 1-based). | 数组实现占 `n + 1` 大小（1 号位起）。 |

---

## 2 · Why the Best-Case Insert Can Be O(1)  
### 为何插入可达 O(1)
 
If the incoming key is *always* ≥ (parent) in a min-heap (or ≤ parent in a max-heap), it never violates the heap property, so no upward swaps occur—just one write at the tail. 
当插入序列与堆序性天然一致（小根堆：严格升序；大根堆：严格降序）时，新节点放在末尾已经符合堆序，不触发任何交换，故时间退化为常数级。

---

## 3 · Priority Queue Overview 优先队列概览

### 3.1 Definition 定义
**EN** — A **Priority Queue (PQ)** delivers elements according to their priority (smallest, largest, earliest, …).  
**中文** — **优先队列**：按照元素“优先级”顺序（最小/最大/自定义）弹出元素的数据结构。

### 3.2 Why Heaps? 为何常用堆

| Implementation | Insert | Extract-Top | 典型场景 |
|---------------|--------|-------------|----------|
| **Binary Heap** | `O(log n)` | `O(log n)` | 主流选择；代码最简；内存局部性好。 |
| d-ary Heap | `O(log_d n)` 插入快 | `O(d log_d n)` | 内核调度、外存 PQ。 |
| Fibonacci Heap | **Amort.** `O(1)` insert | `O(log n)` | 图算法理想（Dijkstra, Prim）；常数大，实践少。 |

---

## 4 · Typical Use-Cases 典型应用举例

| Domain 领域 | English Use-Case | 中文说明 |
|-------------|------------------|----------|
| OS Scheduling | CPU time-slice selection | 进程/线程时间片分配 |
| Networking | Bandwidth / packet prioritization | 路由器带宽或数据包优先级管理 |
| Printing | Printer job queue | 打印任务先后 |
| Airlines | Boarding groups by status | 航班登机分区 |
| Algorithms | Dijkstra, A*, Huffman, Kruskal… | 图/编码/最小生成树等算法中的辅助结构 |

---
