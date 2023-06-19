---
description: Try to solve the 'Search in a Binary Search Tree' problem.
---

# Search in a Binary Search Tree (easy)

## Problem Statement

You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

## Examples

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/12/tree1.jpg)

<pre><code><strong>Input: root = [4,2,7,1,3], val = 2
</strong><strong>Output: [2,1,3]
</strong></code></pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/12/tree2.jpg)

<pre><code><strong>Input: root = [4,2,7,1,3], val = 5
</strong><strong>Output: []
</strong></code></pre>

## **Constraints**

* The number of nodes in the tree is in the range `[1, 5000]`.
* `1 <= Node.val <= 107`
* `root` is a binary search tree.
* `1 <= val <= 107`

## Solution

### Approach 1: Recursive

* If the tree is empty `root == null` or the value to find is here `val == root.val` - return root.
* If `val < root.val` - go to search into the left subtree `searchBST(root.left, val)`.
* If `val > root.val` - go to search into the right subtree `searchBST(root.right, val)`.
* Return `root`.

![Search Traversal](https://leetcode.com/problems/search-in-a-binary-search-tree/Figures/700/recursion.png)

### Approach 2: Iterative

To reduce the space complexity, convert recursive approach into the iterative one:

* While the tree is not empty `root != null` and the value to find is _not_ here `val != root.val`:
  * If `val < root.val` - go to search into the left subtree `root = root.left`.
  * If `val > root.val` - go to search into the right subtree `root = root.right`.
* Return `root`.

![Search Traversal](https://leetcode.com/problems/search-in-a-binary-search-tree/Figures/700/iteration.png)

## Just the Code

### Approach 1: Recursive

{% code title="Solution.java" overflow="wrap" lineNumbers="true" fullWidth="true" %}
```java
class Solution {
  public TreeNode searchBST(TreeNode root, int val) {
    if (root == null || val == root.val) return root;

    return val < root.val ? searchBST(root.left, val) : searchBST(root.right, val);
  }
}
```
{% endcode %}

### Approach 2: Iterative

{% code title="Solution.java" overflow="wrap" lineNumbers="true" fullWidth="true" %}
```java
class Solution {
  public TreeNode searchBST(TreeNode root, int val) {
    while (root != null && val != root.val)
      root = val < root.val ? root.left : root.right;
    return root;
  }
}
```
{% endcode %}

## Summary

### Approach 1: Recursive

#### Time Complexity $$\mathcal{O}(\log N)$$&#x20;

$$O(H)$$, where $$H$$ is a tree height. That results in $$O(log⁡N)$$ in the average case, and $$O(N)$$ in the worst case.

Let's compute time complexity with the help of [master theorem](https://en.wikipedia.org/wiki/Master\_theorem\_\(analysis\_of\_algorithms\)) $$T(N)=aT(\frac{N}{b})+Θ(N^d)$$.\
The equation represents dividing the problem up into $$a$$ subproblems of size $$\frac{N}{b}$$ in $$Θ(N^d)$$ time.\


Here at step there is only one subproblem `a = 1`, its size is a half of the initial problem `b = 2`,\
and all this happens in a constant time `d = 0`, as for the binary search.&#x20;

That means that $$\log_b{a} = d$$ and hence we're dealing with [case 2](https://en.wikipedia.org/wiki/Master\_theorem\_\(analysis\_of\_algorithms\)#Case\_2\_example) that results in $$\mathcal{O}(n^{\log_b{a}} \log^{d + 1} N) = \mathcal{O}(\log N)$$ time complexity.

#### Space Complexity $$\mathcal{O}(H)$$&#x20;

$$\mathcal{O}(H)$$ to keep the recursion stack, ie $$\mathcal{O}(\log N)$$ in the average case, and $$\mathcal{O}(N)$$ in the worst case.

### Approach 2: Iteration

#### Time Complexity $$\mathcal{O}(\log N)$$

$$O(H)$$, where $$H$$ is a tree height. That results in $$O(log⁡N)$$ in the average case, and $$O(N)$$ in the worst case.

Let's compute time complexity with the help of [master theorem](https://en.wikipedia.org/wiki/Master\_theorem\_\(analysis\_of\_algorithms\)) $$T(N)=aT(\frac{N}{b})+Θ(N^d)$$.\
The equation represents dividing the problem up into $$a$$ subproblems of size $$\frac{N}{b}$$ in $$Θ(N^d)$$ time.\


Here at step there is only one subproblem `a = 1`, its size is a half of the initial problem `b = 2`,\
and all this happens in a constant time `d = 0`, as for the binary search.&#x20;

That means that $$\log_b{a} = d$$ and hence we're dealing with [case 2](https://en.wikipedia.org/wiki/Master\_theorem\_\(analysis\_of\_algorithms\)#Case\_2\_example) that results in $$\mathcal{O}(n^{\log_b{a}} \log^{d + 1} N) = \mathcal{O}(\log N)$$ time complexity.

#### Space Complexity $$\mathcal{O}(H)$$

$$\mathcal{O}(1)$$ since it's a constant space solution.



\
