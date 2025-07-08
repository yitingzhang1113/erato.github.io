---
layout: post
title: "birnary tree（二叉树）学习笔记-修改与构造"
date: 2025-06-09
tags: [data structure, 二叉树]
comments: true
author: Erato
permalink: /create-blog-with-github-pages/2025-06-11/
---

 226.翻转二叉树 （优先掌握递归） 
题目链接：[226. Invert Binary Tree](https://leetcode.cn/problems/invert-binary-tree/description/)

```java
/**
 * 递归实现：翻转二叉树（Invert Binary Tree）
 *
 * 核心思路：对每个节点做一次“交换左右子节点”的操作，并将此操作递归地应用到整棵树的每个节点上。
 *
 * 关于遍历顺序：
 *   • 后序遍历（左 → 右 → 中）  
 *       1. 先递归翻转左子树  
 *       2. 再递归翻转右子树  
 *       3. 最后交换当前节点的左右孩子  
 *   • 前序遍历（中 → 左 → 右）  
 *       1. 先交换当前节点的左右孩子  
 *       2. 再递归翻转（此时已交换过左右的）左子树  
 *       3. 最后递归翻转右子树  
 *   两者都是合法的实现方式。
 *
 * 为什么中序（左 → 中 → 右）不行？  
 *   1. 先递归翻转左子树（OK）  
 *   2. 中序到根时交换左右孩子，左右指针互换  
 *   3. 再递归翻转“右子树” —— 但此时右子树已是原左子树，造成对原左子树重复翻转  
 *   导致原右子树只翻转一次、原左子树翻转两次，结果错乱。
 */
public class Solution {
    /**
     * 翻转二叉树的入口
     * @param root 原始二叉树根节点
     * @return 翻转后的二叉树根节点
     */
    public TreeNode invertTree(TreeNode root) {
        // 1）终止条件：遇到空节点直接返回
        if (root == null) {
            return null;
        }

        // —— 可选顺序示例：后序遍历 ↓ ——
        // 2）递归翻转左子树
        invertTree(root.left);
        // 3）递归翻转右子树
        invertTree(root.right);
        // 4）交换当前节点的左右孩子，完成以当前节点为根的子树翻转
        swapChildren(root);

        // 5）返回当前节点引用，保证父节点能正确链接子树
        return root;
    }

    /**
     * 交换指定节点的左右子节点
     * @param node 待交换子节点的父节点
     */
    private void swapChildren(TreeNode node) {
        TreeNode tmp      = node.left;  // 暂存左孩子
        node.left         = node.right; // 将右孩子赋给左
        node.right        = tmp;        // 将原左孩子赋给右
    }
}
```
```java
/**
 * 迭代实现：翻转二叉树（Invert Binary Tree）— 广度优先搜索（BFS）
 *
 * 核心思路：借助队列按层遍历二叉树，每访问到一个节点就交换它的左右孩子，
 * 并将孩子节点继续入队以便后续处理，直到队列为空，所有节点都被处理完毕。
 */
public class Solution {
    public TreeNode invertTree(TreeNode root) {
        // 1）遇到空树，直接返回 null
        if (root == null) {
            return null;
        }

        // 2）使用 ArrayDeque 作为队列，先将根节点入队
        ArrayDeque<TreeNode> deque = new ArrayDeque<>();
        deque.offer(root);

        // 3）只要队列不空，就继续按层处理
        while (!deque.isEmpty()) {
            // （可选）记录当前层的节点数，通常用于“分层”操作
            int size = deque.size();

            // 4）遍历当前层的所有节点
            while (size-- > 0) {
                // 4.1）从队头取出一个节点
                TreeNode node = deque.poll();

                // 4.2）交换该节点的左右孩子
                swap(node);

                // 4.3）如果有左孩子，加入队列，待下一轮处理
                if (node.left != null) {
                    deque.offer(node.left);
                }
                // 4.4）如果有右孩子，同样加入队列
                if (node.right != null) {
                    deque.offer(node.right);
                }
            }
            // 处理完一层后，回到 while(!deque.isEmpty()) 处，继续下一层
        }

        // 5）所有节点都交换完后，返回新的根节点
        return root;
    }

    /**
     * 交换指定节点的左右子节点
     * @param node 待交换孩子的父节点
     */
    private void swap(TreeNode node) {
        TreeNode temp   = node.left;
        node.left       = node.right;
        node.right      = temp;
    }
}
```

 101. 对称二叉树 （优先掌握递归）  
题目链接：[101. Symmetric Tree](https://leetcode.cn/problems/symmetric-tree/description/)

 104.二叉树的最大深度 （优先掌握递归）

什么是深度，什么是高度，如何求深度，如何求高度，这里有关系到二叉树的遍历方式。


 111.二叉树的最小深度 （优先掌握递归）


