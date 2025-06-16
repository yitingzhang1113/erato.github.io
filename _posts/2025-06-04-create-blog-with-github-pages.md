---
layout: post
title: "数据结构之 Array(数组）详解"
date: 2025-06-04
tags: [数组]
comments: true
author: Erato
permalink: /create-blog-with-github-pages/2025-06-04/
---
# 数组与数组列表


---

## 1. 数组定义

- 数组是由相同类型的变量按顺序排列组成的集合。
- 数组中的每个单元格都有一个索引，表示它在数组中的位置。
- 索引唯一对应该单元格中存储的值。
- 数组 `A` 的单元格从索引0开始编号，依次为0, 1, 2，依此类推。
- 数组中存储的每个值通常称为该数组的一个**元素**。

示例索引位置：  
`0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15`

---

## 2. 数组长度与容量

- 数组的长度决定了数组能存储的最大元素数量，也称为数组的**容量**。
- 数组长度在创建时确定且不可更改（固定）。
- 在 Java 中，数组 `a` 的长度用 `a.length` 访问。
- 数组 `a` 的单元格索引从0开始，到 `a.length - 1`。
- 访问索引为 `k` 的元素用 `a[k]`。

示例索引位置：  
`0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15`

---

## 3. 数组创建

- 数组可以用**数组字面量**或**数组声明**两种方式声明。
- 元素类型可以是任何Java基本类型或类类型。

容量为 `N = 4` 的示例：  
```java
// 数组字面量
int[] myArray = {1, 3, 3, 2};

// 数组声明
int[] myArray = new int[4];
## 4. 字符或对象引用数组
```
数组可以存储原始类型元素，如字符。

示例：

| S | A | M | P | L | E |
|---|---|---|---|---|---|
| 0 | 1 | 2 | 3 | 4 | 5 |

数组也可以存储对象引用。

示例：

| "Rene" | "Joseph" | "Janet" | "Jonas" | "Helen" |
|--------|----------|---------|---------|---------|
| 0      | 1        | 2       | 3       | 4       |

---

## 5. 数组列表（Array Lists）

- 数组列表是以数组作为底层结构的动态列表。
- 数组列表存储的是对象，而非原始数据类型。
- 普通数组可以存储原始类型和对象。
- 当数组列表满了，会动态扩容：
  - 创建一个更大的新数组。
  - 将旧数组内容复制到新数组中。
- 扩容策略因实现不同而异。
- Java中，扩容为原大小的1.5倍。

---
动画演示：[ArrayList](https://csvistool.com/ArrayList)


## 6. 添加元素

在数组 `myArray` 的索引 `i` 处添加元素 `e`，需要先将从 `i` 到末尾的元素往后移动，腾出空间。

示例：

- 容量 = 16  
- 当前元素个数 `n = 10`  
- 在索引 `i = 6` 添加元素时，将索引6至9的元素依次往后移一位（变成7至10）。

添加操作伪代码：

```pseudo
procedure Add(i, e)
  if size >= arr.length then
    扩容数组
  end if
  for j ← size − 1 down to i do
    arr[j + 1] ← arr[j]
  end for
  arr[i] ← e
  size ← size + 1
end procedure
```

作业里的
```java
   /**
     * Adds the element to the specified index.
     *
     * Remember that this add may require elements to be shifted.
     *
     * Must be amortized O(1) for index size and O(n) for all other cases.
     *
     * @param index the index at which to add the new element
     * @param data  the data to add at the specified index
     * @throws java.lang.IndexOutOfBoundsException if index < 0 or index > size
     * @throws java.lang.IllegalArgumentException  if data is null
     */
    public void addAtIndex(int index, T data) {
        // check
        if (data == null) {
            throw new IllegalArgumentException("Data cannot be null");
        }
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException("Index out of bounds: " + index);
        }
        /* resize */
        if (size == backingArray.length) {
            T[] newArr = (T[]) new Object[backingArray.length * 2];
            for (int i = 0; i < size; i++) {
                newArr[i] = backingArray[i];
            }
            backingArray = newArr;
        }

        /* Insert */
        if (index == size) {
            backingArray[size] = data;
        } else {
            for (int i = size - 1; i >= index; i--) {
                backingArray[i + 1] = backingArray[i];
            }
            backingArray[index] = data;
        }
        /* Update size */
        size++;
    }

    /**
     * Adds the element to the front of the list.
     *
     * Remember that this add may require elements to be shifted.
     *
     * Must be O(n).
     *
     * @param data the data to add to the front of the list
     * @throws java.lang.IllegalArgumentException if data is null
     */
    public void addToFront(T data) {
        // check
        if (data == null) {
            throw new IllegalArgumentException("Data cannot be null");
        }
        /* resize */
        if (size == backingArray.length) {
            T[] newArr = (T[]) new Object[backingArray.length * 2];
            for (int i = 0; i < size; i++) {
                newArr[i] = backingArray[i];
            }
            backingArray = newArr;
        }
        /* Insert */
        for (int i = size - 1; i >= 0; i--) {
            backingArray[i + 1] = backingArray[i];
        }
        backingArray[0] = data;
        /* Update size */
        size++;
    }

    /**
     * Adds the element to the back of the list.
     *
     * Must be amortized O(1).
     *
     * @param data the data to add to the back of the list
     * @throws java.lang.IllegalArgumentException if data is null
     */
    public void addToBack(T data) {
        // check
        if (data == null) {
            throw new IllegalArgumentException("Data cannot be null");
        }
        /* resize */
        if (size == backingArray.length) {
            T[] newArr = (T[]) new Object[backingArray.length * 2];
            for (int i = 0; i < size; i++) {
                newArr[i] = backingArray[i];
            }
            backingArray = newArr;
        }
        /* Insert */
        backingArray[size] = data;
        /* Update size */
        size++;

    }
```

## 7. 删除元素
删除索引 t 处的元素后，需要将从 t+1 到末尾的元素向前移动，填补空缺。

示例：

容量 = 16

当前元素个数 n = 11


删除操作伪代码：

```pseudo
procedure Remove(i)
  item ← arr[i]
  arr[i] ← NULL
  for j ← i to size − 2 do
    arr[j] ← arr[j + 1]
  end for
  size ← size − 1
  return item
end procedure
```
作业里的
```java
   /**
     * Adds the element to the specified index.
     *
     * Remember that this add may require elements to be shifted.
     *
     * Must be amortized O(1) for index size and O(n) for all other cases.
     *
     * @param index the index at which to add the new element
     * @param data  the data to add at the specified index
     * @throws java.lang.IndexOutOfBoundsException if index < 0 or index > size
     * @throws java.lang.IllegalArgumentException  if data is null
     */
    public void addAtIndex(int index, T data) {
        // check
        if (data == null) {
            throw new IllegalArgumentException("Data cannot be null");
        }
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException("Index out of bounds: " + index);
        }
        /* resize */
        if (size == backingArray.length) {
            T[] newArr = (T[]) new Object[backingArray.length * 2];
            for (int i = 0; i < size; i++) {
                newArr[i] = backingArray[i];
            }
            backingArray = newArr;
        }

        /* Insert */
        if (index == size) {
            backingArray[size] = data;
        } else {
            for (int i = size - 1; i >= index; i--) {
                backingArray[i + 1] = backingArray[i];
            }
            backingArray[index] = data;
        }
        /* Update size */
        size++;
    }

    /**
     * Adds the element to the front of the list.
     *
     * Remember that this add may require elements to be shifted.
     *
     * Must be O(n).
     *
     * @param data the data to add to the front of the list
     * @throws java.lang.IllegalArgumentException if data is null
     */
    public void addToFront(T data) {
        // check
        if (data == null) {
            throw new IllegalArgumentException("Data cannot be null");
        }
        /* resize */
        if (size == backingArray.length) {
            T[] newArr = (T[]) new Object[backingArray.length * 2];
            for (int i = 0; i < size; i++) {
                newArr[i] = backingArray[i];
            }
            backingArray = newArr;
        }
        /* Insert */
        for (int i = size - 1; i >= 0; i--) {
            backingArray[i + 1] = backingArray[i];
        }
        backingArray[0] = data;
        /* Update size */
        size++;
    }

    /**
     * Adds the element to the back of the list.
     *
     * Must be amortized O(1).
     *
     * @param data the data to add to the back of the list
     * @throws java.lang.IllegalArgumentException if data is null
     */
    public void addToBack(T data) {
        // check
        if (data == null) {
            throw new IllegalArgumentException("Data cannot be null");
        }
        /* resize */
        if (size == backingArray.length) {
            T[] newArr = (T[]) new Object[backingArray.length * 2];
            for (int i = 0; i < size; i++) {
                newArr[i] = backingArray[i];
            }
            backingArray = newArr;
        }
        /* Insert */
        backingArray[size] = data;
        /* Update size */
        size++;

    }
```

leetcode 【27. Remove Element](tps://leetcode.cn/problems/remove-element/description/)
## 8. 数组列表总结与时间复杂度
数组列表是动态的，存储对象。

访问元素的时间复杂度：
O(1)，常数时间。

在除数组尾部以外的位置插入、搜索或删除元素的时间复杂度：
O(n)，线性时间。

数组列表的实际应用示例：在线游戏地图中跟踪角色。

# 递归 (Recursion)
---

## 1. 递归定义

递归是一种编程过程，其中一个方法不断调用自身，直到达到定义的终止条件。

---

## 2. 递归示例

经典示例：阶乘函数  
- 计算公式：  
  \( n! = 1 \times 2 \times 3 \times \cdots \times (n-1) \times n \)

递归定义：  
\[
f(n) = 
\begin{cases}
1 & \text{if } n=0 \\
n \times f(n-1) & \text{else}
\end{cases}
\]

Java实现示例：

```java
public static int factorial(int n) {
    if (n < 0) {
        throw new IllegalArgumentException("参数必须非负");
    } else if (n == 0) {
        return 1; // 基本情况
    } else {
        return n * factorial(n - 1); // 递归调用
    }
}
```
## 3. 递归方法构造
递归方法包括：

基本情况（Base case）：参数值满足某条件时，不再递归调用。

递归调用（Recursive calls）：明确调用自身，参数逐渐趋近基本情况。

要求：

每个递归方法至少有一个基本情况。

所有递归调用链最终必须达到基本情况。

递归调用要保证参数朝向基本情况推进。

## 4. 递归可视化
递归调用过程可以用图示表示：

每个递归调用对应一个盒子。

箭头表示调用关系（调用者→被调用者）。

返回箭头表示返回值的传递。

举例：计算 factorial(4)，递归调用栈展开如下：

## 5. 二分查找方法
leetcdoe ：[704. Binary Search](https://leetcode.cn/problems/binary-search/description/)


## 📌 二分查找适用前提

在使用二分法前，请确保你的题目满足以下条件：

- ✅ **数组有序**
- ✅ **无重复元素（有时允许重复元素，但返回规则需明确）**

这些前提条件决定了我们可以利用“对半剪枝”的策略，有效提升查找效率。

---

## 🚨 常见误区：为什么写不好二分？

很多人写不好二分查找，是因为：

- 搞不清楚 `left <= right` 和 `left < right` 的区别
- 搞不清楚 `right = mid - 1` 还是 `right = mid`
- 对“**区间定义**”和“**循环不变量**”没有建立正确的认知

---

## 📐 思路核心：明确区间定义！

### 两种常用定义：

| 区间写法       | 区间表示      | 说明                         |
|----------------|----------------|------------------------------|
| `[left, right]` | 左闭右闭区间  | 包含 `left` 和 `right` 两端 |
| `[left, right)` | 左闭右开区间  | 包含 `left`，不包含 `right` |

**⚠️ 关键：一旦定义了区间类型，就必须严格按照定义更新边界！**

---

## 🧪 写法一：左闭右闭 `[left, right]`

### ✨ 特点：

- `while (left <= right)`：当 `left == right` 时依然是有效区间
- 更新 `right = mid - 1`，`left = mid + 1`

### ✅ 代码（Java）：

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1; // 定义 [left, right]

        while (left <= right) {
            int mid = left + ((right - left) >> 1); // 防止溢出
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;  // target 在右区间
            } else {
                right = mid - 1; // target 在左区间
            }
        }

        return -1; // 没找到
    }
}
 时间复杂度：O(log n)
 空间复杂度：O(1)
写法二：左闭右开 [left, right)
✨ 特点：
while (left < right)：当 left == right 时表示无效区间

更新 right = mid（不是 mid - 1）

代码（Java）：
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length; // 定义 [left, right)

        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;  // target 在右区间
            } else {
                right = mid;     // target 在左区间
            }
        }

        return -1; // 没找到
    }
}
```
🔁 推荐刷题

Leetcode [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/description/)

Leetcode [34. 查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

Leetcode [69. x 的平方根](https://leetcode.cn/problems/sqrtx/description/)

Leetcode [367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/description/)


## 6. 二分查找示例
考虑三种情况：

如果 target == data[mid]，找到目标，返回True。

如果 target < data[mid]，递归搜索左半区间。

如果 target > data[mid]，递归搜索右半区间。

## 7. 二分查找可视化
蓝色区域表示当前搜索区间。

绿色元素为当前比较元素。

例如搜索19：

Copy
数据序列：
2 4 5 7 8 9 12 14 17 19 22 25 27
逐步缩小搜索区间，直到找到目标或确定不存在。

## 8. 二分查找时间复杂度分析

- 运行时间为 \(O(\log n)\)。
- 每次递归调用将搜索区间缩小一半。
- 因此最多递归层数为 \(\log n\)。

---

## 9. 计算幂函数示例

幂函数定义：

\[
p(a, n) = 
\begin{cases}
1 & \text{if } n = 0 \\
a \times p(a, n-1) & \text{else}
\end{cases}
\]

- 此递归算法时间复杂度为 \(O(n)\)，因为递归调用次数为 \(n\)。

---

## 10. 递归平方法

使用重复平方提升幂运算效率：

\[
p(a, n) =
\begin{cases}
1 & n=0 \\
a \times p\left(a, \frac{n-1}{2}\right)^2 & n \text{为奇数} \\
p\left(a, \frac{n}{2}\right)^2 & n \text{为偶数}
\end{cases}
\]

示例计算：

\[
2^4 = \left(2^{4/2}\right)^2 = (2^2)^2 = 4^2 = 16
\]

\[
2^5 = 2 \times \left(2^{\frac{5-1}{2}}\right)^2 = 2 \times (2^2)^2 = 2 \times 16 = 32
\]

---

## 11. 递归平方法伪代码

```pseudo
procedure Power(a, n)
  if n = 0 then
    return 1
  end if
  if n is odd then
    y ← Power(a, (n - 1)/2)
    return a × y × y
  else
    y ← Power(a, n/2)
    return y × y
  end if
end procedure
```
## 12. 尾递归
尾递归是一种特殊的递归形式，可以被编译器优化为迭代，避免递归带来的开销。

特点：

每个递归调用是函数的最后一步。

不需要保存调用栈帧，因为结果直接返回。

可能获得运行时的优化（但 Java 不做此优化）。

## 其他leetcode知识点
## 双指针[代码随想录]（https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE）
977.有序数组的平方
力扣题目链接(opens new window)

给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

示例 1：

输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]，排序后，数组变为 [0,1,9,16,100]
示例 2：

输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
#
leetcode [977. Squares of a Sorted Array](https://leetcode.cn/problems/squares-of-a-sorted-array/description/)
