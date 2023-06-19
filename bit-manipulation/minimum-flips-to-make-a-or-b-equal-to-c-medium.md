---
description: Try to solve the 'Minimum Flips to Make a OR b Equal to c' problem.
---

# Minimum Flips to Make a OR b Equal to c (medium)

## Problem Statement

Given 3 positives numbers `a`, `b` and `c`. Return the minimum flips required in some bits of `a` and `b` to make ( `a` OR `b` == `c` ). (bitwise OR operation).\
Flip operation consists of change **any** single bit 1 to 0 or change the bit 0 to 1 in their binary representation.

## Examples

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/06/sample\_3\_1676.png)

<pre><code><strong>Input: a = 2, b = 6, c = 5
</strong><strong>Output: 3
</strong><strong>Explanation: After flips a = 1 , b = 4 , c = 5 such that (a OR b == c)
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: a = 4, b = 2, c = 7
</strong><strong>Output: 1
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: a = 1, b = 2, c = 3
</strong><strong>Output: 0
</strong></code></pre>

## **Constraints**

* `1 <= a <= 10^9`
* `1 <= b <= 10^9`
* `1 <= c <= 10^9`

## Solution

### **Intuition **_**"Bit Manipulation"**_

To determine the minimum number of bit flips required in `a` and `b` to achieve their bitwise OR equal to `c`, we can manipulate each bit of `a` and `b`. This can be implemented by iterating over the bits of the numbers from the least significant (the rightmost) bit to the most significant (the leftmost) bit.

> To obtain the least significant bit of `P`, we can use bitwise AND operator `P & 1`

During the iteration, we will keep track of `answer`, the number of bit flips required in `a` and `b`. For each bit position, we need to consider two cases:

* Case 1: `(c & 1) = 1`: In this case, we need at least one bit of `1` in either `a & 1` or `b & 1`. If either `a & 1` or `b & 1` is `1`, we can move on to the next bit, otherwise, we need to flip a bit in either `a` or `b`.
* Case 2: `(c & 1) = 0`: In this case, both `a & 1` and `b & 1` should be equal to `0`, if either `a & 1` or `b & 1` is equal to `1`, we need one flip to make it equal to `0`. Therefore, the number of flips equals the sum of `a & 1` and `b & 1`.

After each bit position, we update `answer` with the number of bit flips required in `a` and `b`. We then shift the bits of `a`, `b` and `c` to the right to check the next bit.

> After shifting `a` to the right (`a >>= 1`), `a & 1` will represent the second least significant bit.

We repeat the above process until all numbers are equal to `0`, and return `answer`.

\


Please refer to the following example, where the least significant bits are shown in the box.

Start with initializing `answer = 0` and examining the least significant bits, we find that `(a & 1) = 0`, `(b & 1) = 0`, and `(c & 1) = 1`, which correspond to case 1 previously mentioned. Thus we need one flip on the least significant bit in either `a` or `b`.

![img](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/Figures/1318/2.png)

We then shift all numbers to the right and examine the second least significant bits. We observe that `(a & 1) = 1`, `(b & 1) = 1`, and `(c & 1) = 0`, which is consistent with case 2, we need to flip both the least significant bits of `a` and `b`, requiring a total of two flips.

![img](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/Figures/1318/3.png)

We then shift all numbers to the right and examine the next bits, which are `(a & 1) = 0`, `(b & 1) = 1` and `(c & 1) = 1`. However, since `(b & 1) = 1`, we do not need to flip any bits this time.

![img](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/Figures/1318/4.png)

All numbers are equal to `0` after shifting them to the right, so we can return `answer = 3`.

### **Algorithm**

1. Initialize a variable `answer` as 0, which will be used to keep track of the minimum number of flips needed.
2. Iterate over each bit of the binary representation of `a`, `b`, and `c` simultaneously:
3. If `(c & 1) = 0`, update answer as `answer += (a & 1) + (b & 1)`.
4. If `(c & 1) = 1`, if both `a & 1` and `b & 1` equal `0`, increment `answer` by 1.
5. Shift all numbers to the right by `a >>= 1`, `b >>= 1`, `c >>= 1`. If all numbers are equal to `0`, return `answer`, otherwise, repeat steps 3 and 4.

## Just the Code

{% code title="Solution.java" overflow="wrap" lineNumbers="true" %}
```java
class Solution {
    public int minFlips(int a, int b, int c) {
        int answer = 0;
        while (a != 0 | b != 0 | c != 0) {
            if ((c & 1) == 1) {
                if ((a & 1) == 0 && (b & 1) == 0) {
                    answer++;
                }
            } else {
                answer += (a & 1) + (b & 1);
            }
            
            a >>= 1;
            b >>= 1;
            c >>= 1;
        }
        
        return answer;
    }
}
```
{% endcode %}

## Summary

Let $$n$$ be the maximum length in the binary representation of `a`, `b` or `c`.

### Time complexity $$O(n)$$

* We need to iterate over the bits of the numbers. Note that we have $$nnn$$ as the number of bits, which is logarithmic with the actual values.

### Space complexity $$O(1)$$

* We only need to update `answer` and modify `a`, `b` and `c`.
