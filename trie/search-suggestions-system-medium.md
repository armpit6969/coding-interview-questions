---
description: Try to solve the 'Search Suggestions System' problem.
---

# Search Suggestions System (medium)

## Problem Statement

You are given an array of strings `products` and a string `searchWord`.

Design a system that suggests at most three product names from `products` after each character of `searchWord` is typed. Suggested products should have common prefix with `searchWord`. If there are more than three products with a common prefix return the three lexicographically minimums products.

Return _a list of lists of the suggested products after each character of_ `searchWord` _is typed_.

## Examples

**Example 1:**

<pre data-full-width="true"><code><strong>Input: products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"
</strong><strong>Output: [["mobile","moneypot","monitor"],["mobile","moneypot","monitor"],["mouse","mousepad"],["mouse","mousepad"],["mouse","mousepad"]]
</strong><strong>Explanation: products sorted lexicographically = ["mobile","moneypot","monitor","mouse","mousepad"].
</strong>After typing m and mo all products match and we show user ["mobile","moneypot","monitor"].
After typing mou, mous and mouse the system suggests ["mouse","mousepad"].
</code></pre>

**Example 2:**

<pre data-full-width="true"><code><strong>Input: products = ["havana"], searchWord = "havana"
</strong><strong>Output: [["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]
</strong><strong>Explanation: The only word "havana" will be always suggested while typing the search word.
</strong></code></pre>

## **Constraints**

* `1 <= products.length <= 1000`
* `1 <= products[i].length <= 3000`
* `1 <= sum(products[i].length) <= 2 * 104`
* All the strings of `products` are **unique**.
* `products[i]` consists of lowercase English letters.
* `1 <= searchWord.length <= 1000`
* `searchWord` consists of lowercase English letters.

## Solution

### **Intuition **_**"Trie + DFS"**_

Whenever we come across questions with multiple strings, it is best to think if [Trie](https://en.wikipedia.org/wiki/Trie) can help us. What we need here is a way to search for all the words with given prefix, this is a well known problem that trie can solve. The question also asks for a sorted results, if you look closely a trie word is represented by it's preorder traversal. It is also worth noting that a preorder traversal of a trie will always result in a sorted traversal of results, thus all we need to do is limit the word traversal to 3.

**Questions using Trie:**

[79. Word Search](https://leetcode.com/problems/word-search)

[211. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure)

![diff](https://leetcode.com/problems/search-suggestions-system/Figures/1268/Trie.png)\
_Figure 1. A trie made from `words`_

### **Algorithm**

1. Create a Trie from the given products input.
2. Iterate each character of the `searchWord` adding it to the `prefix` to search for.
3. After adding the current character to the `prefix` traverse the `trie` pointer to the node representing `prefix`.
4. Now traverse the tree from `curr` pointer in a preorder fashion and record whenever we encounter a complete word.
5. Limit the result to 3 and return `dfs` once reached this limit.
6. Add the words to the final result.

## Just the Code

{% code title="Trie.java" overflow="wrap" lineNumbers="true" fullWidth="true" %}
```java
// Custom class Trie with function to get 3 words starting with given prefix
class Trie {

    // Node definition of a trie
    class Node {
        boolean isWord = false;
        List<Node> children = Arrays.asList(new Node[26]);
    };
    Node Root, curr;
    List<String> resultBuffer;

    // Runs a DFS on trie starting with given prefix and adds all the words in the resultBuffer, limiting result size to 3
    void dfsWithPrefix(Node curr, String word) {
        if (resultBuffer.size() == 3)
            return;
        if (curr.isWord)
            resultBuffer.add(word);

        // Run DFS on all possible paths.
        for (char c = 'a'; c <= 'z'; c++)
            if (curr.children.get(c - 'a') != null)
                dfsWithPrefix(curr.children.get(c - 'a'), word + c);
    }
    Trie() {
        Root = new Node();
    }

    // Inserts the string in trie.
    void insert(String s) {

        // Points curr to the root of trie.
        curr = Root;
        for (char c : s.toCharArray()) {
            if (curr.children.get(c - 'a') == null)
                curr.children.set(c - 'a', new Node());
            curr = curr.children.get(c - 'a');
        }

        // Mark this node as a completed word.
        curr.isWord = true;
    }
    List<String> getWordsStartingWith(String prefix) {
        curr = Root;
        resultBuffer = new ArrayList<String>();
        // Move curr to the end of prefix in its trie representation.
        for (char c : prefix.toCharArray()) {
            if (curr.children.get(c - 'a') == null)
                return resultBuffer;
            curr = curr.children.get(c - 'a');
        }
        dfsWithPrefix(curr, prefix);
        return resultBuffer;
    }
};
class Solution {
    List<List<String>> suggestedProducts(String[] products,
                                         String searchWord) {
        Trie trie = new Trie();
        List<List<String>> result = new ArrayList<>();
        // Add all words to trie.
        for (String w : products)
            trie.insert(w);
        String prefix = new String();
        for (char c : searchWord.toCharArray()) {
            prefix += c;
            result.add(trie.getWordsStartingWith(prefix));
        }
        return result;
    }
};

```
{% endcode %}

## Summary

### Time Complexity $$O(M)$$

Let `M` is total number of characters in `products` in the `trie`. For each `prefix` we find its representative node in $$O(len(prefix))$$ and DFS to find at most 3 words which is an `O(1)` operation. Thus the overall complexity is dominated by the time required to build the `trie`.

In Java there is an additional complexity of $$O(m^2)$$ due to Strings being immutable, here `m` is the length of `searchWord`.

### Space complexity $$O(26n)=O(n)$$

Here `n` is the number of nodes in the `trie`. `26` is the alphabet size.&#x20;

Space required for output is $$O(m)$$ where `m` is the length of the search word.
