---
description: >-
  Let's solve the 'Merge Strings Alternately' problem using the Two Pointers
  pattern.
---

# Merge Strings Alternately (easy)

## Problem Statement

You are given two strings `word1` and `word2`. Merge the strings by adding letters in alternating order, starting with `word1`. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return _the merged string._

## Examples

**Example 1:**

<pre><code><strong>Input: word1 = "abc", word2 = "pqr"
</strong><strong>Output: "apbqcr"
</strong><strong>Explanation: The merged string will be merged as so:
</strong>word1:  a   b   c
word2:    p   q   r
merged: a p b q c r
</code></pre>

**Example 2:**

<pre><code><strong>Input: word1 = "ab", word2 = "pqrs"
</strong><strong>Output: "apbqrs"
</strong><strong>Explanation: Notice that as word2 is longer, "rs" is appended to the end.
</strong>word1:  a   b 
word2:    p   q   r   s
merged: a p b q   r   s
</code></pre>

**Example 3:**

<pre><code><strong>Input: word1 = "abcd", word2 = "pq"
</strong><strong>Output: "apbqcd"
</strong><strong>Explanation: Notice that as word1 is longer, "cd" is appended to the end.
</strong>word1:  a   b   c   d
word2:    p   q 
merged: a p b q c   d
</code></pre>

## **Constraints**

* `1 <= word1.length, word2.length <= 100`
* `word1` and `word2` consist of lowercase English letters.

## Solution

### **Intuition**

There are numerous ways in which we can combine the given strings. We've covered a few of them in this article.

An intuitive method is to use two pointers to iterate over both strings. Assume we have two pointers, `i` and `j`, with `i` pointing to the first letter of `word1` and `j` pointing to the first letter of `word2`. We also create an empty string `results` to store the outcome.

We append the letter pointed to by pointer `i` i.e., `word1[i]`, and increment `i` by `1` to point to the next letter of `word1`. Because we need to add the letters in alternating order, next we append `word2[j]` to `results`. We also increase `j` by `1`.

We continue iterating over the given strings until both are exhausted. We stop appending letters from `word1` when `i` reaches the end of `word1`, and we stop appending letters from `word2'` when `j` reaches the end of `word2`.

Here's a visual representation of how the approach works in the second example given in the problem description:

### **Algorithm**

1. Create two variables, `m` and `n`, to store the length of `word1` and `word2`.
2. Create an empty string variable `result` to store the result of merged words.
3. Create two pointers, `i` and `j` to point to indices of `word1` and `word2`. We initialize both of them to `0`.
4. While `i < m || j < n`:
   * If `i < m`, it means that we have not completely traversed `word1`. As a result, we append `word1[i]` to `result`. We increment `i` to point to next index of `words`.
   * If `j < n`, it means that we have not completely traversed `word2`. As a result, we append `word2[j]` to `result`. We increment `j` to point to next index of `words`.
5. Return `results`.

It is important to note how we form the `result` string in the following codes:\
\- `cpp`: The strings are mutable in cpp, which means they can be changed. As a result, we used the `string` variable and performed all operations on it. It takes constant time to append a character to the string.\
\- `java`: The `String` class is immutable in java. So we used the mutable `StringBuilder` to concatenate letters to `result`.\
\- `python`: Strings are immutable in python as well. As a result, we used the list `result` to append letters and later joined the list with an empty string to return it as a string object. The `join` operation takes linear time equal to the length of `results` to merge `results` with empty string.

## Just the Code

{% code title="Solution.java" overflow="wrap" lineNumbers="true" fullWidth="true" %}
```java
class Solution {
    public String mergeAlternately(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        StringBuilder result = new StringBuilder();
        int i = 0, j = 0;

        while (i < m || j < n) {
            if (i < m) {
                result.append(word1.charAt(i++));
            }
            if (j < n) {
                result.append(word2.charAt(j++));
            }
        }

        return result.toString();
    }
}
```
{% endcode %}

## **Summary**

Here, $$m$$ is the length of `word1` and $$n$$ is the length of `word2`.

### **Time Complexity** $$O(m + n)$$

* We iterate over `word1` and `word2` once and push their letters into `result`. It would take $$O(m+n)$$ time.

### **Space Complexity** $$O(1)$$

Without considering the space consumed by the input strings (`word1` and `word2`) and the output string (`result`), we do not use more than constant space.

\
