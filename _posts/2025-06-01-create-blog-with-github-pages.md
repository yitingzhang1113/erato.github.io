---
layout: post
title: "2025年暑期学习计划 (6.10 – 8.10)"
date: 2025-06-01
tags: [学习计划]
comments: true
author: Erato
permalink: /create-blog-with-github-pages/2025-06-01/
---
## 一、数据结构（Data Structures）

> **目标**：补足 5.12–6.6 的核心内容并在 8.10 前完成后续学习与刷题。  
> **方法**：结合 Lecture Modules、教材章节、Homework & Problem Set，以及 LeetCode 刷题。
---

## 紧急补课计划：5.12–6.13 全部内容（6.10–6.15 完成）

> **策略**：从 Week 5（最新）往前补，到 Week 1；每天混合多个主题，高效率覆盖所有内容。

| 日期       | 学习内容                                                                                                                                                |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **6/10 周二** | **Week 5 → Week 4**<br/>•  Module 4：Binary Trees，Module 6：BST Operations + Binary Heaps + SkipLists=<br/>• AVL properties + Single rotations<br/>• Module 7：Chapter 9.1–9.2<br/>• Maps & HashTables |
| **6/11 周三** | **Week 4 → Week 3**<br/>• Module 7：Chapter 9.3–9.4, 10.1–10.2<br/>• HashMaps & External Chaining<br/>• Module 6：Chapter 8 & Chapter 11.1<br/>• BST Traversals |
| **6/12 周四** | **Week 3 → Week 2**<br/>• Module 8：Chapter 10.4 & Chapter 11.3<br/>• Module 4：Stacks (复习 & 练习)<br/>• Module 3：Queues & Deques (Chapter 8 & 11.1) |
| **6/13 周五** | **Week 2 → Week 1**<br/>•<br/>• Module 2：Chapter 1–2 + Syllabus & Reality Check<br/>• ArrayLists (Java 实现练习)                |
| **6/14 周六** | **Week 1**<br/>• Module 2/3：Chapter 3.1–3.4 + Chapter 6.1–6.2<br/>• LinkedLists（单向/循环/双向）<br/>• Recursion（理论 & 代码实践）                        |
| **6/15 周日** | **Week 1 余下 + 总复习**<br/>• Iterators & Iterable<br/>• 整理笔记 & 错题本<br/>• LeetCode 混合挑战 2–3 道（ArrayList、LinkedList、Recursion、Tree 等）         |

### Week 6：2–4 树 (Module 9, Chapter 12.1)  
> **目标**：掌握 2–4 树的节点结构、插入时的 Overflow 分裂和删除时的 Underflow 合并  

| 日期       | 内容                                                                                 | 任务                                                                                           |
| ---------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| **6/16 周一** | • 阅读 Chapter 12.1 前半：2–4 树节点结构 & Properties<br/>• 理解“满节点”与“最少键数”       | - 手绘 2–4 树节点布局<br/>- 在纸上演示 3 键节点插入后 Overflow 分裂                              |
| **6/17 周二** | • 插入算法 & Overflow 机制<br/>• 分裂后如何把中间键上移                                   | - 用 Java/C++ 实现 `insert(key)` 基本框架<br/>- 编写 3–4 个小例子验证分裂逻辑                      |
| **6/18 周三** | • 删除算法 & Underflow 机制<br/>• 从兄弟节点借键 vs 合并节点                              | - 手写 delete(key) 的借键与合并流程<br/>- 在纸上画出 Underflow 合并前后节点变化                   |
| **6/19 周四** | **学校假日**                                                                         | - 复习周一至周三笔记<br/>- 在 LeetCode 上找 “B-Tree Insert” 风格题，写出思路（不用提交）           |
| **6/20 周五** | • Underflow 代码实现<br/>• 完整插入+删除流程整合                                         | - 在代码中补全合并逻辑<br/>- LeetCode “Design B+ Tree” 或 “Implement B-tree” 模拟练习             |

---

### Week 7：排序算法 (Module 10 & 11, Chapter 12.1 & 12.3)  
> **目标**：熟练掌握迭代排序与分治排序的原理、实现与优化  

| 日期       | 内容                                                                                 | 任务                                                                                           |
| ---------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| **6/23 周一** | • Selection Sort, Insertion Sort, Bubble Sort 原理<br/>• 时间 & 空间复杂度分析           | - 手写三种算法伪代码<br/>- LeetCode “Easy” 级别排序题（如 Sort Colors）                          |
| **6/24 周二** | • Merge Sort<br/>• 分治思想：分、治、合                                                  | - 实现 `mergeSort(arr)`<br/>- 在随机数组上测试，并对比 Java 内置 `Arrays.sort()`                 |
| **6/25 周三** | • Quick Sort<br/>• 选取 pivot 策略 & 分区算法                                          | - 实现三种 pivot 选取（首/中/随机）<br/>- 分析最坏、平均、最好情况时间复杂度                     |
| **6/26 周四** | • Heap Sort<br/>• Heapify & 堆排序流程                                                  | - 用数组模拟最大堆<br/>- 实现 `heapSort(arr)` 并测试                                           |
| **6/27 周五** | • 综合练习：多算法对比                                                                 | - LeetCode “Merge Intervals” / “Sort Characters By Frequency”<br/>- 总结每种算法优缺点           |

---

### Week 8：模式匹配 (Module 12, Chapter 12.2 & 12.5, +Paper)  
> **目标**：掌握 Boyer–Moore 和 KMP 算法  

| 日期       | 内容                                                                                 | 任务                                                                                           |
| ---------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| **6/30 周一** | • 模式匹配基础：暴力 vs 优化策略<br/>• 了解坏字符 & 好后缀规则                        | - 手动模拟一次 BM 坏字符移动<br/>- 列出好后缀表构造步骤                                         |
| **7/1 周二**  | • Boyer–Moore 详细流程<br/>• 坏字符表 & 好后缀表构建                                 | - 实现 `boyerMoore(text, pat)`<br/>- 在几个样例串上测试并打印每次移动位置                           |
| **7/2 周三**  | • KMP 算法：前缀函数 (π) 计算<br/>• 利用 π 表优化匹配                                  | - 实现 `computePrefix(pat)`<br/>- 实现 `kmpSearch(text, pat)`                                    |
| **7/3 周四**  | **学校假日**                                                                         | - 复习 BM & KMP 代码<br/>- LeetCode “Implement strStr()” 思路化练习                             |
| **7/4 周五**  | • 模式匹配综合练习                                                                   | - LeetCode “Minimum Window Substring”<br/>- LeetCode “Wildcard Matching”                         |

---

### Week 9：字符串 & 图论 (Module 13 & 14, Chapter 14.1–14.3 + Paper)  
> **目标**：掌握 Rabin–Karp 哈希 & 基本图算法  

| 日期       | 内容                                                                                 | 任务                                                                                           |
| ---------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| **7/7 周一**  | • Galil Rule 概述<br/>• Rabin–Karp 滚动哈希思想                                       | - 实现 `rabinKarp(text, pat)`<br/>- 在长文本中测试 hash 冲突                                    |
| **7/8 周二**  | • Rabin–Karp 练习<br/>• 分析哈希冲突处理                                             | - LeetCode “Implement strStr()” 哈希解法<br/>- 总结 vs KMP 优劣                                |
| **7/9 周三**  | • 图的存储 (邻接表/矩阵)<br/>• BFS & DFS 理论与代码                                    | - 实现 `bfs(start)` & `dfs(start)`<br/>- 在小图示例上跑通                                    |
| **7/10 周四** | • 图遍历练习                                                                         | - LeetCode “Number of Islands”<br/>- LeetCode “Course Schedule”                                 |
| **7/11 周五** | • Dijkstra’s 最短路径算法<br/>• 优先队列实现                                         | - 实现 `dijkstra(src)`<br/>- LeetCode “Network Delay Time” 或 “Path With Minimum Effort”         |

---

### Week 10：MST & LCS (Module 15 & 16)  
> **目标**：掌握最小生成树算法 & 最长公共子序列  

| 日期         | 内容                                                                               | 任务                                                                                         |
| ------------ | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **7/14 周一**  | • MST 概念 & Prim’s Algorithm<br/>• 贪心策略                                       | - 实现 `primMST()`<br/>- 在加权无向图示例上测试                                            |
| **7/15 周二**  | • Kruskal’s Algorithm<br/>• 并查集 (Union-Find)                                   | - 实现并测试 `kruskalMST()`<br/>- 完善并查集路径压缩                                        |
| **7/16 周三**  | • LCS 动态规划方程 & 状态转移                                                     | - 实现 `longestCommonSubsequence(s1, s2)`<br/>- 打印 DP 表                                      |
| **7/17 周四**  | • 综合练习                                                                         | - LeetCode “Minimum Spanning Tree”<br/>- LeetCode “Edit Distance”                            |
| **7/18 周五**  | • 周总结 & 弱点复盘                                                                 | - 整理错题本<br/>- 制作思维导图<br/>- 制定下一阶段刷题计划                                      |

---

> **每日练习建议**：  
> - **Lecture/Reading**：上午 1–2h 专注阅读与笔记  
> - **Coding**：下午 1–2h 实现 & 测试  
> - **LeetCode**：晚间 30–60min 刷 1–2 题，务必字节级优化  
> - **复盘**：当天结束前 15min 回顾，记录疑问与收获

## 二、Full Stack 开发

> **目标**：掌握前端 → React → 后端整合 → 完成全栈项目  
> **课程链接**：  
> - **Udemy—Complete Web Development Bootcamp**：  
>   https://www.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12638830#overview  

1. **6.10 – 6.20**：HTML/CSS、JavaScript 基础与项目  
2. **6.21 – 7.5**：React 组件、状态管理、Router  
3. **7.6 – 7.20**：Node.js + Express + 数据库；RESTful API  
4. **7.21 – 8.5**：全栈项目（React + 后端 + 部署）  
5. **8.6 – 8.10**：项目优化、README & 部署文档、Demo 准备

---

## 三、AI Engineer（基于 RAG）

> **目标**：理解 RAG 原理 → 搭建检索 → 集成生成 → 完成 RAG Demo  
> **课程链接**：  
> - **Udemy—The AI Engineer Course**：  
>   https://www.udemy.com/course/the-ai-engineer-course-complete-ai-engineer-bootcamp/  

1. **6.10 – 6.20**：RAG 概念、Tokenization、Embeddings  
2. **6.21 – 7.5**：检索模块（Elasticsearch/FAISS）  
3. **7.6 – 7.20**：生成模型集成（Hugging Face / OpenAI）  
4. **7.21 – 8.5**：端到端 RAG 应用（FastAPI + React）  
5. **8.6 – 8.10**：性能调优、项目报告 & Demo 准备

## 我准备的project

## 1. 个人网站的搭建

### 1.1 原型设计  
- 基于 [avatar-blue-dreams-website](https://github.com/yitingzhang1113/avatar-blue-dreams-website) 完成前端原型  
  - 主页、关于我、项目展示等静态页面  
  - 响应式布局，支持手机/平板/桌面  

### 1.2 后端架构  
- 技术栈  
  - **框架**：Next.js API Routes 或 Node.js + Express  
  - **数据库**：  
    - Neo4j（知识图谱）  
    - MongoDB（用户交互日志）  
- API 设计  
  - RESTful 或 GraphQL 接口  
  - 身份验证：JWT  

### 1.3 AI 数字人形象设计  
- 借鉴 [Duix.Heygem](https://github.com/duixcom/Duix.Heygem)  
  - 导入三维卡通模型，绑定面部表情和手势动画  
  - 制作我的 AI 头像 —— 卡通风格、可定制表情  

### 1.4 知识图谱集成  
- 在 Neo4j 中建模：  
  - **节点**：转行经历、实习求职、项目列表、思想理念  
  - **关系**：时间线（`:NEXT`）、主题归属（`:ABOUT`）  
- 使用 Cypher 查询，在对话中动态检索相关节点  

### 1.5 多模态交互  
- **语音交互**  
  - 录制并训练我的语音样本，用于 TTS（文本转语音）和 ASR（语音识别）  
  - Web Speech API + 深度学习模型，支持中／英双语切换  
- **文本对话**  
  - 集成 OpenAI GPT-4 接口，理解自然语言、生成回答  
  - 前端使用 React 实时渲染对话窗  

### 1.6 项目展示  
- 将所有项目部署并链接到 “项目” 页面：  
  1. [avatar-blue-dreams-website](https://github.com/yitingzhang1113/avatar-blue-dreams-website)  
  2. [Duix.Heygem](https://github.com/duixcom/Duix.Heygem)  
  3. …（补充其他核心项目）  
- 每个项目卡片：简要说明 + GitHub 链接 + 截图  

### 1.7 部署与运维  
- **平台**：Vercel 或 Netlify  
- **CI/CD**：GitHub Actions 自动化构建 & 部署  
- **域名**：购买个人域名，配置 HTTPS  

### 1.8 时间规划  

| 阶段       | 时间范围   | 主要任务                           |
| ---------- | ---------- | ---------------------------------- |
| 原型优化   | Week 1     | 完善前端交互、响应式布局            |
| 后端搭建   | Week 2     | 搭建基础 API、配置数据库            |
| AI 数字人  | Week 3–4   | 集成模型、训练语音 & 表情动画       |
| 知识图谱   | Week 5     | 构建 Neo4j 图谱、测试检索逻辑       |
| 多模态交互 | Week 6     | 语音/文本接口调试及前端集成         |
| 部署上线   | Week 7     | CI/CD 配置、域名绑定、正式发布      |


## 2.准备一个java后端的有我不太会的知识的project

---

**Erato**  
计算机科学爱好者  
[GitHub](https://github.com/yourusername) | [个人博客](https://yourusername.github.io)
