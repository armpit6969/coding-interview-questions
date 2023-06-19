---
description: Try to solve the 'Kth Largest Element in an Array' problem.
---

# Kth Largest Element in an Array (medium)

## Problem Statement

Given an integer array `nums` and an integer `k`, return the `kth` _largest element_ in the array.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

You must solve it in `O(n)` time complexity.

## Examples

**Example 1:**

<pre><code><strong>Input: nums = [3,2,1,5,6,4], k = 2
</strong><strong>Output: 5
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
</strong><strong>Output: 4
</strong></code></pre>

## **Constraints**

* `1 <= k <= nums.length <= 105`
* `-104 <= nums[i] <= 104`

## Solution

A heap is a very powerful data structure that allows us to efficiently find the maximum or minimum value in a dynamic dataset.

If you are not familiar with heaps, we recommend checking out the [Heap Explore Card](https://leetcode.com/explore/learn/card/heap/).

The problem is asking for the $$k^{th}$$ largest element. Let's push all the elements onto a min-heap, but pop from the heap when the size exceeds `k`. When we pop, the smallest element is removed. By limiting the heap's size to `k`, after handling all elements, the heap will contain exactly the $$k$$ largest elements from the array.

\


<figure><img src="https://leetcode.com/problems/kth-largest-element-in-an-array/Figures/215/1.png" alt=""><figcaption><p>If Heap Size exceeds k, remove element and continue adding elements to heap</p></figcaption></figure>

It is impossible for one of the green elements to be popped because that would imply there are at least $$k$$ elements in the array greater than it. This is because we only pop when the heap's size exceeds `k`, and popping removes the smallest element.

After we handle all the elements, we can just check the top of the heap. Because the heap is holding the $$k$$ largest elements and the top of the heap is the smallest element, the top of the heap would be the $$k^{th}$$ largest element, which is what the problem is asking for.

**Algorithm**

1. Initialize a min-heap `heap`.
2. Iterate over the input. For each `num`:
   * Push `num` onto the heap.
   * If the size of `heap` exceeds `k`, pop from `heap`.
3. Return the top of the `heap`.

**Implementation**

> Note: C++ [std::priority\_queue](https://en.cppreference.com/w/cpp/container/priority\_queue) implements a max-heap. To achieve min-heap functionality, we will multiply the values by `-1` before pushing them onto the heap.

## Just the Code

{% code title="Solution.java" overflow="wrap" lineNumbers="true" fullWidth="true" %}
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        for (int num: nums) {
            heap.add(num);
            if (heap.size() > k) {
                heap.remove();
            }
        }
        
        return heap.peek();
    }
}
```
{% endcode %}

## Summary

Given $$n$$ as the length of `nums`,

### Time Complexity $$O(n⋅log⁡ k)$$

Operations on a heap cost logarithmic time relative to its size. Because our heap is limited to a sizeof `k`, operations cost at most $$O(log⁡k)$$. We iterate over `nums`, performing one or two heap operations at each iteration. We iterate $$n$$ times, performing up to $$log⁡ k$$ work at each iteration, giving us a time complexity of $$O(n⋅log⁡k)$$.

Because $$k≤nk$$, this is an improvement on the '**Sort**' approach.

### Space Complexity $$O(k)$$

The heap uses $$O(k)$$ space.
