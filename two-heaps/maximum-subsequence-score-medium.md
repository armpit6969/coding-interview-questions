---
description: Try to solve the 'Maximum Subsequence Score' problem.
---

# Maximum Subsequence Score (medium)

## Problem Statement

You are given two **0-indexed** integer arrays `nums1` and `nums2` of equal length `n` and a positive integer `k`. You must choose a **subsequence** of indices from `nums1` of length `k`.

For chosen indices `i0`, `i1`, ..., `ik - 1`, your **score** is defined as:

* The sum of the selected elements from `nums1` multiplied with the **minimum** of the selected elements from `nums2`.
* It can defined simply as: `(nums1[i0] + nums1[i1] +...+ nums1[ik - 1]) * min(nums2[i0] , nums2[i1], ... ,nums2[ik - 1])`.

Return _the **maximum** possible score._

A **subsequence** of indices of an array is a set that can be derived from the set `{0, 1, ..., n-1}` by deleting some or no elements.

## Examples

**Example 1:**

<pre><code><strong>Input: nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
</strong><strong>Output: 12
</strong><strong>Explanation: 
</strong>The four possible subsequence scores are:
- We choose the indices 0, 1, and 2 with score = (1+3+3) * min(2,1,3) = 7.
- We choose the indices 0, 1, and 3 with score = (1+3+2) * min(2,1,4) = 6. 
- We choose the indices 0, 2, and 3 with score = (1+3+2) * min(2,3,4) = 12. 
- We choose the indices 1, 2, and 3 with score = (3+3+2) * min(1,3,4) = 8.
Therefore, we return the max score, which is 12.
</code></pre>

**Example 2:**

<pre><code><strong>Input: nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
</strong><strong>Output: 30
</strong><strong>Explanation: 
</strong>Choosing index 2 is optimal: nums1[2] * nums2[2] = 3 * 10 = 30 is the maximum possible score.
</code></pre>

## **Constraints**

* `n == nums1.length == nums2.length`
* `1 <= n <= 105`
* `0 <= nums1[i], nums2[j] <= 105`
* `1 <= k <= n`

## Solution

#### Approach: Priority Queue <a href="#approach-priority-queue" id="approach-priority-queue"></a>

**Intuition**

Start with a brute force approach, if we find the maximum score by checking all groups of indexes with size `k`, there are $${n \choose k} = {{n!}\over{k! (n - k)!}}$$ possibilities. We can't afford to check them one by one.

Let's first focus on the minimum of the selected elements from `nums2`. If we pick `nums2[i]` as the minimum, it means the other `k - 1` selected elements from `nums2` are larger or equal to `nums2[i]`.

We can thus take advantage of this restriction on the selection by sorting `nums2`, which can reduce the time complexity. Assume that we have sorted `nums2` by decreasing order (Note that we can't change the relative order of `nums1` and `nums2`, so its better to store each pair as `(nums1[i], nums2[i])` and sort the collection of pairs according to `nums2[i]`).

As shown in the picture below, if we pick `nums2[i]` (colored in red) as the minimum selected element from `nums2`, we can freely select the rest `k - 1` indexes to the left of `i` without changing the second term: minimum of the selected elements from `nums2`.

![img](https://leetcode.com/problems/maximum-subsequence-score/Figures/2542/2.png)

Recall the definition of the score, the second term has been fixed as `nums2[i]`, so we can maximzie the total score by maximizing the first term, that is, by selecting the maximum `k` elements from `nums1` including `nums1[i]`.

![img](https://leetcode.com/problems/maximum-subsequence-score/Figures/2542/3.png)

This can be done efficiently by maintaining a min-heap that always contains the largest `k` elements we have seen. Whenever we pick a new `nums2[i]` as the minimum from `nums2`, we shall remove one element from the heap (which represents removing a `nums1` number and add `nums[i]` to it. Now the heap contains the largest `k` element including `nums1[i]` again, the current score equals the sum of this heap times `nums2[i]`.

We can iterate over `nums2` and repeat the above process. At each step, we calculate the current score and update `answer` as the maximum score we have met.

Take the following slides as an example:

\


**Algorithm**

1. Store every pair `(nums1[i], nums2[i])` in array `pairs`, and sort `pairs` by the second element (`nums2[i]`) in decreasing order.
2. Use a min-heap `top_k_heap` to store the first `k` `nums1[i]` and a variable `top_k_sum` to store their sum.
3. Initialize `answer` as the sum of elements in `top_k_heap` (i.e. `top_k_sum`) times `pairs[k - 1][1]`.
4. Iterate over indices starting from `k`, at each index `i`:
   * Remove the smallest element stored in `top_k_heap` and from `top_k_sum`.
   * Add the current `nums1[i]` to the heap and `top_k_sum`.
   * Get the current score as the sum of `top_k_heap` (i.e. `top_k_sum`) times `nums2[i]`, and update `answer` as the maximum score we have met.
5. Return `answer`.

## Just the Code

## Summary

**Complexity Analysis**

Let $$n$$ be the length of the input array `nums1`.

### Time Complexity $$O(n⋅log⁡n)$$

* We need to sort `nums2`, it takes $$O(n⋅log⁡n)$$.
* Then we iterate over `pairs` of length `n`. At each iteration step `i`, we remove the smallest element from `top_k_heap` and add one element `pairs[i][0]` to it. Both the inserting and removing operations to priority queue of size $$k$$ take $$O(log⁡k)$$ time.
* To sum up, the overall time complexity is $$O(n⋅log⁡n)$$, because $$k≤nk$$.

### Space Complexity $$O(n)$$

* We store every pair `(nums1[i], nums2[i])` in a 2-d array `pairs`, it takes $$O(n)$$ space.
* The in-place sorting method also uses some additional space, in Python, it uses $$O(n)$$ space and in Java, it uses $$O(log⁡n)$$ space.
* The priority queue contains at most $$k$$ elements thus it takes $$O(k)$$ space.
