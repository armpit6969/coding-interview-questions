---
description: Try to solve the 'Leaf-Similar Trees' problem.
---

# Leaf-Similar Trees (easy)

## Problem Statement

Consider all the leaves of a binary tree, from left to right order, the values of those leaves form a **leaf value sequence**_._

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png)

For example, in the given tree above, the leaf value sequence is `(6, 7, 4, 9, 8)`.

Two binary trees are considered _leaf-similar_ if their leaf value sequence is the same.

Return `true` if and only if the two given trees with head nodes `root1` and `root2` are leaf-similar.

## Examples

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-1.jpg)

<pre><code><strong>Input: root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
</strong><strong>Output: true
</strong></code></pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-2.jpg)

<pre><code><strong>Input: root1 = [1,2,3], root2 = [1,3,2]
</strong><strong>Output: false
</strong></code></pre>

## **Constraints**

* The number of nodes in each tree will be in the range `[1, 200]`.
* Both of the given trees will have values in the range `[0, 200]`.

Solution

Just the Code

Summary
