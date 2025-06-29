---
layout: post
title: "hashmap"
date: 2025-06-14
tags: [java]
comments: true
author: Erato
permalink: /create-blog-with-github-pages/2025-06-14/
---
# HashMaps 简介 / Introduction to HashMaps
哈希表（HashMap）通过 **散列函数** 将键映射到整数，再存入数组结构，从而在平均情况下实现 `O(1)` 的查找、插入与删除。  
*A HashMap turns each key into an integer via a **hash function** and stores the pair in an array-backed table, giving average-case `O(1)` lookup, insert, and delete.*

---

## 1  映射与集合 ADT  /  Map vs. Set ADT

| ADT | 存储内容 | 主要操作 (示例) | ADT | What it stores | Core ops (sample) |
|-----|----------|-----------------|-----|----------------|-------------------|
| **Map** | 唯一不可变的 **键 → 值** | `put(k,v)`, `remove(k)`, `get(k)`, `containsKey(k)` | **Map** | Unique, immutable **key → value** | `put(k,v)`, `remove(k)`, `get(k)`, `containsKey(k)` |
| **Set** | 仅存键，无值 | `add(k)`, `remove(k)`, `contains(k)` | **Set** | Keys only (no values) | `add(k)`, `remove(k)`, `contains(k)` |

> *若需要维持顺序，可使用 **Ordered Map**，但普通 HashMap 不保证顺序*  
> *Ordered variants (e.g., `LinkedHashMap`) maintain key order; plain HashMap does not.*

---

## 2  散列函数 `h : K → ℤ`  /  Hash Functions `h : K → ℤ`

**最小要求**：`k₁ = k₂ ⇒ h(k₁) = h(k₂)`  
*Minimum requirement: equal keys produce equal hashes.*

**理想但通常做不到**：`h(k₁) = h(k₂) ⇒ k₁ = k₂`  
*Desirable but impossible in general: equal hashes imply equal keys.*

### 字符串键示例 / Example (hashing a `String`)
```text
简单法：A=1,B=2,... → "ABAB" = 1+2+1+2 = 6   ← 次序信息丢失，冲突多  
改进法：按位权重 (×10ⁿ) → 1·10³ + 2·10² + 1·10¹ + 2·10⁰ = 1222
```

## 3  `hashCode()` 与 `equals` 合同 / The `hashCode()`–`equals` Contract

- **相等对象** (`equals` 返回 `true`) **必须** 拥有相同 `hashCode`。  
  *Equal objects must share the same hash.*

- **不相等对象** 仍可能得到相同散列值 → **冲突 / collision**。  
  *Collisions are inevitable; aim to minimise them.*

---

##  4  散列之外的三要素 / Three More Design Choices

| 要素 | 作用 | 常见做法 | Aspect | Purpose | Typical choice |
|------|------|----------|--------|---------|----------------|
| **压缩函数** | 将散列值映射到 `[0, tableLen−1]` | `index = hash % tableLen` | Compression | Fold raw hash into table range | `index = hash % tableLen` |
| **表长** | 配合压缩函数均匀分布键 | 质数 或 `2ⁿ` | Table size | Help spread keys | Prime or power-of-two |
| **装填因子** `α = size / capacity` | 扩容阈值 | 0.5 – 0.75 | Load factor | Resize threshold | 0.5 – 0.75 |

> 装填因子低 → 更少冲突，但更占内存；装填因子高 → 更省内存，但冲突增多。  
> *Lower `α` means fewer collisions but uses more memory; higher `α` saves memory but increases collisions.*


## 5 冲突解决概览 / Collision-Resolution Overview
链地址法 Separate chaining — 每个槽存链表 / 动态数组 / 树

开放定址法 Open addressing — 线性探测 (linear), 二次探测 (quadratic), 双重散列 (double hashing)

## 6 示例：重写 hashCode / Example: Overriding hashCode
```java
public class Vehicle {
    private double mpg, maxSpeed;
    private int numWheels;
    private String name;

    public Vehicle(double mpg, double maxSpeed, int numWheels, String name) {
        this.mpg = mpg;
        this.maxSpeed = maxSpeed;
        this.numWheels = numWheels;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Vehicle v)) return false;
        return mpg == v.mpg
            && maxSpeed == v.maxSpeed
            && numWheels == v.numWheels
            && name.equals(v.name);
    }

    @Override
    public int hashCode() {
        int prime = 31, res = 1;
        res = prime * res + Double.hashCode(mpg);
        res = prime * res + Double.hashCode(maxSpeed);
        res = prime * res + numWheels;
        res = prime * res + name.hashCode();
        return res;
    }
}
```
## 5  碰撞处理策略 / Collision-Handling Policies

HashMap 中的碰撞处理大体分为两类：  
*Collision handling in hash tables falls into two broad families:*

| 类型 / Category | 定义 / Definition | 常见实现 / Popular variant |
|-----------------|------------------|---------------------------|
| **Closed Addressing**<br>封闭寻址 | 冲突键保存在“原槽”内的某个附属结构 | **External Chaining** (链地址法) |
| **Open Addressing**<br>开放寻址 | 冲突键可存放到表中其他槽，位置由探测序列决定 | Linear / Quadratic probing, Double hashing |

---

### 5.1  外部链地址法 / External Chaining (Closed Addressing)

* 在每个数组槽中维护一个链表 / 动态数组 / 平衡树来存放所有冲突键。  
* 查找、插入、删除都局部完成在该链结构中。  
* 负载因子可安全超过 1，但链过长会拖慢操作。

---

### 5.2  线性探测 / Linear Probing (Open Addressing)

> **基本思想 / Idea**  
> 如果槽 `i` 被占，用步长 1 继续探测：`(i+1) % tableLen`, `(i+2) % tableLen`, …，直到找到空槽或目标键。

#### 5.2.1  插入 put(k,v)

1. 计算索引 `idx = compress(hash(k))`  
2. 若槽空 → 直接放入。  
3. 否则连续向右探测，直到找到**首个**空槽或同键槽进行覆盖。

#### 5.2.2  查找 / 搜索 get(k)

1. 同样从 `idx` 开始线性探测。  
2. 遇到  
   * **空槽 (`null`)** → 终止，表示未找到；  
   * **DEL 标记** → 跳过继续探测；  
   * **匹配键** → 返回值；  
   * **其他键** → 继续探测。

#### 5.2.3  删除 remove(k) — *软删除 / soft removal*

- **不能**直接清空槽，否则破坏探测序列。  
- 改为将条目标记为 **DEL**（又称 *tombstone*），占位但对用户不可见。

```text
┌───────────────┐
│ key,value,DEL │  ← 标记位区分活动/删除状态
└───────────────┘
```
### 5.3  DEL 标记的效率影响 / Efficiency of DEL Markers

#### 终止条件 (未命中时) / Termination Conditions (Key Not Found)

1. **探测到 `null` 槽**  
   *Probe hits a `null` slot → stop.*
2. **已探测 `tableLen` 次（环绕一圈）**  
   *Probed `tableLen` positions (full wrap-around) → stop.*

> 若 DEL 过多且无 `null`，查找将扫描整张表 → **`O(n)`**  
> Too many DELs with no `null` => every search may scan the entire table (linear time).

---

#### 极端示例 / Pathological Example

| 步骤 | 操作 | 结果 |
|------|------|------|
| 表长 = 13 | — | — |
| 1–13 | 依次插入并删除键 `0‥12` | 表内仅剩 13 个 **DEL** 标记；无 `null` |
| 装填因子 | 始终 ≤ 0.08 | 但所有操作之后 **无空槽** |
| 后续任何 put/get/remove | 必须探测 13 次 | 性能退化为 `O(n)` |

---

#### 5.3.1  缓解策略 / Mitigations

- **将 DEL 槽计入负载因子**，触发更早扩容  
  *Include DEL slots when computing load factor to force earlier resize.*

- **定期重构 / rehash**，丢弃所有 DEL 标记  
  *Periodically rehash the table, discarding tombstones.*

- 在**预计大量删除**的场景，**主动重建表结构**  
  *Proactively rebuild the table when you expect heavy deletions.*

