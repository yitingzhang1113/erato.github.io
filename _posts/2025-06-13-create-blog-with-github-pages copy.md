---
layout: post
title: "java base"
date: 2025-06-13
tags: [java]
comments: true
author: Erato
permalink: /create-blog-with-github-pages/2025-06-13/
---

## 📌 一、Java 是按值传递（Pass By Value）

### ✅ 什么是“按值传递”？

Java 中所有的参数传递方式都是 **Pass By Value（按值传递）**，即：调用方法时，会把变量的**值**拷贝一份传进去。

- 如果是 **基本类型（primitive）**，复制的是具体的数值。
- 如果是 **引用类型（对象）**，复制的是对象在内存中的“地址值”（引用）。

---

### 🧪 示例 1：基本类型

```java
public static void main(String[] args) {
    int count = 0;
    helper(count);
    System.out.println(count);  // 输出：0
}

public static void helper(int x) {
    x = x + 1;
}
```
解释：

count 的值是 0，传给 helper 时复制了一份 0 给 x

修改 x 不会影响 count

✅ 修改方案（返回值）
```java
public static void main(String[] args) {
    int count = 0;
    count = helper(count);
    System.out.println(count);  // 输出：1
}

public static int helper(int x) {
    return x + 1;
}
```
🧪 示例 2：对象类型
```java
class Container {
    private int item;

    public Container(int item) {
        this.item = item;
    }

    public void setItem(int item) {
        this.item = item;
    }

    public int getItem() {
        return item;
    }
}
```
```java
public static void main(String[] args) {
    Container c = new Container(0);
    helper(c);
    System.out.println(c.getItem());  // 输出：1
}

public static void helper(Container x) {
    x.setItem(x.getItem() + 1);  // 修改的是原对象
}
```
解释：

c 是引用变量，传给 helper 时复制了“地址值”

所以 x 和 c 指向同一个对象

修改对象内部属性会影响原对象

⚠️ 再看一个误区示例：
```java
public static void main(String[] args) {
    Container c = new Container(0);
    helper(c);
    System.out.println(c.getItem());  // 输出：0
}

public static void helper(Container x) {
    x = new Container(1);  // 修改的是 x 本身的引用，不影响原对象
}
```
🧠 总结：参数传递行为对比
类型	是否影响原变量	说明
基本类型	❌	值复制
对象引用 - 修改对象属性	✅	指向同一内存地址
对象引用 - 修改对象引用	❌	改变的是副本的引用

🎯 二、Java 泛型（Generics）
✅ 为什么需要泛型？
使用泛型前，我们通常写如下类：

```java
public class Container {
    private Object obj;

    public void set(Object obj) { this.obj = obj; }
    public Object get() { return obj; }
}
```
使用时必须强制类型转换，容易出错：

```java
Container c = new Container();
c.set("hello");
Integer i = (Integer) c.get();  // ⚠️ Runtime error: ClassCastException
```
📦 使用泛型（Generic）
```java
public class Container<T> {
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```
使用示例：

```java
Container<String> c1 = new Container<>();
c1.set("hello");
String s = c1.get();  // ✅ 类型安全，无需强转
```
🔧 泛型语法总结
1. 泛型类定义
```java
public class Box<T> {
    private T item;
    public void set(T item) { this.item = item; }
    public T get() { return item; }
}
```
2. 泛型类使用
```java
Box<Integer> box = new Box<>();
box.set(123);
int val = box.get();
```
📐 泛型数组的处理
Java 不允许直接创建泛型数组：

```java
T[] arr = new T[10];  // ❌ 错误
```
解决方法：

```java
T[] arr = (T[]) new Object[10];  // ✅ 可用但有警告
```
🔁 多个类型参数
```java
class Pair<K, V> {
    private K key;
    private V value;
}
```
使用：

```java
Pair<String, Integer> pair = new Pair<>();
```
🔗 嵌套泛型
```java
ArrayList<Container<String>> list = new ArrayList<>();
```
## Iterators 

### 1. 为什么需要迭代器 (Why Iterators?)
- **多样的数据结构**  
  数组、`ArrayList`、链表、树、哈希表……各种数据结构都可能需要遍历全部元素。  
- **避免“为每种结构写一套遍历代码”**  
  迭代器（Iterator）把“逐个访问节点”这一操作抽象出来，提供统一接口，隐藏底层实现差异。  

---

### 2. 迭代器的概念 (What Is an Iterator?)

| 关键点 | 说明 |
| ------ | ---- |
| **本质** | 一个专门的对象/类，用来顺序访问数据结构中的元素。 |
| **抽象能力** | 提供少量通用方法（如 `next()`、`hasNext()`），屏蔽具体数据结构的细节。 |
| **常见能力** | 取得当前元素、移动到下一个/上一个元素。 |

---

### 3. Java 中的两大接口

| 接口 | 所在包 | 主要方法 | 作用 |
| ---- | ------ | -------- | ---- |
| **`Iterator<E>`** | `java.util` | `hasNext()` → 是否还有元素<br>`next()` → 返回下一个元素<br>`remove()` → 删除最近一次返回的元素（可选实现） | 负责遍历逻辑 |
| **`Iterable<T>`** | `java.lang` | `iterator()` → 返回一个 `Iterator` 实例 | 让数据结构“可迭代”，支持 `for-each` 语法 |

---

### 5. 使用迭代器时的注意事项 (Caveats)
结构修改并发问题
遍历期间修改底层容器（增删元素）可能触发
ConcurrentModificationException，具体行为取决于实现。

remove() 方法
并非所有迭代器都支持；若未实现直接调用将抛出 UnsupportedOperationException。

一次性扫描语义
大多数迭代器只能前向遍历且只遍历一次；要再次遍历需重新获取迭代器。


### 6. 在链表中实现自定义迭代器
```java
public class LinkedList<T> implements Iterable<T> {

    /* 其他字段 / 方法省略 */

    /** 返回一个指向链表头结点的迭代器 */
    @Override
    public Iterator<T> iterator() {
        return new LinkedListIterator<>(head);
    }

    /** 链表节点定义（内部类） */
    private static class Node<E> {
        E data;
        Node<E> next;
        Node(E data, Node<E> next) {
            this.data = data;
            this.next = next;
        }
    }

    /** 迭代器实现（内部静态类） */
    private static class LinkedListIterator<E> implements Iterator<E> {
        private Node<E> currentNode;

        LinkedListIterator(Node<E> startingNode) {
            currentNode = startingNode;
        }

        @Override
        public boolean hasNext() {
            return currentNode != null;
        }

        @Override
        public E next() {
            if (!hasNext()) throw new NoSuchElementException();
            E data = currentNode.data;
            currentNode = currentNode.next;
            return data;
        }

        /* remove() 可选实现 */
        @Override
        public void remove() {
            throw new UnsupportedOperationException();
        }
    }
}
```

### 7. 迭代器的使用
```java
Iterator<Integer> llIterator = linkedList.iterator();

while (llIterator.hasNext()) {        // 只有 hasNext() 为 true 时才调用 next()
    Integer data = llIterator.next();
    System.out.println("Data: " + data);
}
```

### 8.  for-each（增强 for 循环）
语法：Java 背后仍然获取 iterator()，再不断调用 hasNext() / next()。

优点：更简洁、减少错误（不会忘记先检查 hasNext()）。

要求：容器类必须 implements Iterable<T> 并返回合法的 Iterator<T>。

```java
for (Integer item : linkedList) {   // 自动调用 iterator()
    System.out.println(item);
}
```
4. Iterable 接口回顾
接口	关键方法	说明
java.lang.Iterable<T>	Iterator<T> iterator()	唯一方法：返回该对象的迭代器实例。

```java
Iterable<String> someList = /* ... */;
Iterator<String> itr = someList.iterator();  // 手动获取
for (String s : someList) {                  // 或使用 for-each
    System.out.println(s);
}
```
### 9. 性能分析

| 数据结构      | 直接 `get(i)` 逐项访问 | 迭代器遍历          | 对比         |
|---------------|-----------------------|----------------------|--------------|
| `ArrayList`   | `O(1) × n → O(n)`    | `O(1) × n → O(n)`   | 相同         |
| `LinkedList`  | `O(n) × n → O(n²)`   | `O(1) × n → O(n)`   | **迭代器更优** |

**结论**

- 对顺序随机访问快的结构（如数组 / `ArrayList`），迭代器与索引遍历复杂度一致。  
- 对顺序随机访问慢的结构（如 `LinkedList`），迭代器能显著降低时间复杂度。

---

### 10. 小结

自定义容器应：

1. `implements Iterable<T>`  
2. 提供内部迭代器类，实现 `Iterator<T>`。

常见遍历写法：

- **显式**  
  ```java
  Iterator<T> it = collection.iterator();
  while (it.hasNext()) {
      T x = it.next();
      // ...
  }
```
- **隐式** 
for (T x : collection) {
    // ...
}
```
## Analysis of Algorithms
### Running Time / 运行时间

- The algorithms considered here will **transform input objects into output objects** for the most part.   此处讨论的算法大多会把**输入对象转换为输出对象**。  

- The **runtime** of an algorithm is the period during which the computer is executing, and it typically **grows with the input size**.  ** **运行时间**是计算机执行算法所花费的时间，通常**随输入规模增大而增长**。  

- The **average-case runtime** may be difficult to determine, and **best-case runtime** is not really considered. **平均情况运行时间**往往难以计算，而**最佳情况运行时间**一般不作重点分析。  

- Our main focus in this course is the **worst-case runtime (Big-O)** of algorithms.  
  本课程主要关注算法的**最坏情况运行时间（Big-O 表示法）**。  
  - Easier to analyze and approximate.  更容易分析与估算。  
  - Crucial to applications such as **games, finance, and robotics**.  
    ** 对于**游戏、金融、机器人**等应用场景尤为关键。  


### Growth of Functions


| 增长级别 (Growth) | Big-O 表示 | Java 代码示例 | 说明 |
|-------------------|------------|---------------|------|
| **Constant**      | `O(1)`     | ```java\nint first = arr[0];\n``` | 直接随机访问；与 `n` 无关 |
| **Logarithmic**   | `O(log n)` | ```java\nint x = n;\nwhile (x > 1) {\n    x /= 2;   // 二分缩减\n}\n``` | 典型：二分查找、堆操作 |
| **Linear**        | `O(n)`     | ```java\nint sum = 0;\nfor (int v : arr) {\n    sum += v;\n}\n``` | 单层遍历；次数随 `n` 成正比 |
| **N-Log-N**       | `O(n log n)` | ```java\nArrays.sort(arr);  // TimSort≈MergeSort / 快排\n``` | 高效比较类排序；外层分治 + 内层线性合并 |
| **Quadratic**     | `O(n²)`    | ```java\nfor (int i = 0; i < n; i++) {\n    for (int j = 0; j < n; j++) {\n        // ...\n    }\n}\n``` | 双重嵌套循环；矩阵运算、朴素字符串匹配 |
| **Cubic**         | `O(n³)`    | ```java\nfor (int i=0;i<n;i++)\n  for (int j=0;j<n;j++)\n    for (int k=0;k<n;k++) {\n        // ...\n    }\n``` | 三重嵌套；Floyd-Warshall 等 |
| **Exponential**   | `O(2ⁿ)`    | ```java\nint fib(int k) {\n    if (k <= 1) return k;\n    return fib(k-1) + fib(k-2); // 重叠子问题，指数爆炸\n}\n``` | 全枚举/回溯无剪枝；子集、全排列、斐波那契递归 |

> **说明**  
> - *Slope* 表示在双对数坐标系中函数曲线的斜率；对 `2ⁿ` 这类指数函数，曲线并非直线，因此用 “曲线” 表示。  
> - 算法分析时常用上述 7 种函数来描述 **最坏情况** 复杂度（Big-O）。
![function](/images/function.png)
### Primitive Operations / 原始操作

- **Basic computations performed by an algorithm** — 算法执行的最基本计算  
- **Identifiable in pseudocode** — 在伪代码中可清晰标识  
- **Largely independent from the programming language** — 与具体编程语言几乎无关  

**Examples / 示例**

1. Evaluating an expression — 计算表达式  
2. Assigning a value to a variable — 给变量赋值  
3. Indexing into an array — 访问数组元素  
4. Calling a method — 调用方法  
5. Returning from a method — 从方法返回  
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
