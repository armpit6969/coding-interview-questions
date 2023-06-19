---
description: Try to solve the 'Valid Palindrome' problem.
---

# Valid Palindrome

## Problem Statement <a href="#statement" id="statement"></a>

Write a function that takes a string, `s`, as an input and determines whether or not it is a palindrome.

> **Note**: A **palindrome** is a word, phrase, or sequence of characters that reads the same backward as forward.

## **Constraints**

<div>

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 194200.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2023-06-12 194230.png" alt=""><figcaption></figcaption></figure>

</div>

## Examples <a href="#examples" id="examples"></a>

* 1  ≤ 1 ≤ `s.length` ≤ 2 \* 10^5
* The string `s` will not contain any white space and will only consist of ASCII characters.

Implement your solution in `main.java` in the following coding template.

```java
import java.util.*;
public class Main{
    public static boolean isPalindrome(String s) {
        // Write your code here
        // Tip: You may use the code template provided
        // in the TwoPointers.java file
        return false;
    }
}
```

## Solution <a href="#solution" id="solution"></a>

So far, you’ve probably brainstormed some approaches and have an idea of how to solve this problem. Let’s explore some of these approaches and figure out which one to follow based on considerations such as time complexity and any implementation constraints.

### Naive Approach <a href="#naive-approach" id="naive-approach"></a>

The naive approach to solve this problem is to reverse the string and then compare the reversed string with the original string. If they match, the original string is a valid palindrome. Although this solution has a linear time complexity, it requires extra space to store the reversed string, making it less efficient in terms of space complexity. Therefore, we can use an optimized approach to save extra space.

### Optimized Approach Using Two Pointers <a href="#optimized-approach-using-two-pointers" id="optimized-approach-using-two-pointers"></a>

A palindrome is a word or phrase that reads the same way when it is reversed. This means that the characters at both ends of the word or phrase should be exactly the same.

The two-pointers approach would allow us to solve this problem in linear time, without any additional space complexity or the use of built-in functions. This is because we’ll traverse the array from the start and the end simultaneously to reach the middle of the string.

> **Note:** In the following section, we will gradually build the solution. Alternatively, you can skip straight to [just the code](https://www.educative.io/courses/grokking-coding-interview-patterns-java/gk2JZw3nXm6#Just-the-code).

### **Step-by-Step Solution**

We’ll have two pointers, where the first pointer is at the starting element of our string, while the second pointer is at the end of the string. We move the two pointers towards the middle of the string and, at each iteration, we compare each element. The moment we encounter a nonidentical pair, we can return FALSE because our string can’t be a palindrome.

We will construct the solution step by step and the first step is to set up two pointers and move them toward the middle of the string. We can do that with the following code snippet:

{% code title="ValidPalindrome1.java" overflow="wrap" lineNumbers="true" fullWidth="true" %}
```java
class ValidPalindrome {

  public static String isPalindrome(String s) {
    System.out.println("String to check: " + s + ". Length of string: " + s.length());
    int left = 0;
    int right = s.length() - 1;
    int i = 1;
    // The terminating condition for the loop is when both the pointers reach the same element or when they cross each other.
    while (left < right) {
      System.out.println("In iteration " + i + ", left = " + left + ", right = " + right);
      System.out.println("The current element being pointed to by the left pointer is '" + s.charAt(left) + "'");
      System.out.println("The current element being pointed to by the right pointer is '" + s.charAt(right) + "'");
      left = left + 1; // Heading towards the right
      right = right - 1; // Heading towards the left
      i = i + 1;
      System.out.println(new String(new char[100]).replace('\0', '-'));
    }
    System.out.println("Loop terminated with left = " + left + ", right = " + right);
    return "The pointers have either reached the same index, or have crossed each other, hence we don't need to look further.";
  }

  //Driver code
  public static void main(String[] arg) {
    String[] testCase = {
      "RACECAR",
      "ABBA",
      "TART"
    };
    for (int k = 0; k < testCase.length; k++) {
      System.out.println("Test Case # " + (k + 1));
      System.out.println(isPalindrome(testCase[k]));
      System.out.println(new String(new char[100]).replace('\0', '-'));
    }
  }
}
```
{% endcode %}

That's how we always traverse in opposite directions with the help of two pointers. The termination condition for our code is that the `left` pointer should always be less than the `right` pointer, because the moment they cross each other, we reach the middle of the string and don't need to go any further. We can check how this works with our example.

In the code sample above, we see that in the case of the palindromic strings, at each step in the traversal toward the middle of the string, the characters at both the left and the right indexes are identical. However, with the third test case, “TART” (which isn't a palindromic string), in the second iteration of the loop, we see that our output identifies that the characters at the left and right indexes aren't the same. This observation allows us to add a simple check to our code.

If we encounter a nonidentical pair, we can simply return FALSE, because the string isn't a palindrome, and we don't need to test any further. Otherwise, we're able to traverse to the middle of the string. In this case, we return TRUE, because each element has a match at the expected position in the string.

{% code title="ValidPalindrome2.java" lineNumbers="true" fullWidth="true" %}
```java
class ValidPalindrome {

  public static boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    System.out.println("The element being pointed to by the left pointer is '" + s.charAt(left) + "'");
    System.out.println("The element being pointed to by the right pointer is '" + s.charAt(right) + "'");
    while (left < right) {
      System.out.println("We check if the two elements are indeed the same, in this case...");
      if (s.charAt(left) != s.charAt(right)) // If the elements at index left and index right are not equal,
      {
        System.out.println("The elements aren't the same, hence we return False");
        return false; // then the symmetry is broken, the string is not a palindrome
      }
      System.out.println("They are the same, thus we move the two pointers toward the middle to continue the \nverification process.\n");
      left = left + 1; // Heading towards the right
      right = right - 1; // Heading towards the left
      System.out.println("The new element at the left pointer is " + s.charAt(left));
      System.out.println("The new element at the right pointer is " + s.charAt(right));
    }
    // We reached the middle of the string without finding a mismatch, so it is a palindrome.
    return true;
  }
  
  //Driver code
  public static void main(String[] arg) {
    String[] testCase = {
      "RACEACAR",
      "A",
      "ABCDEFGFEDCBA",
      "ABC",
      "ABCBA",
      "ABBA",
      "RACEACAR"
    };
    for (int k = 0; k < testCase.length; k++) {
      System.out.println("Test Case #" + (k + 1));
      System.out.println(new String(new char[100]).replace('\0', '-'));
      System.out.println("The input string is " + testCase[k] + "' and the length of the string is " + testCase[k].length() + ".");
      System.out.println("\nIs it a palindrome?..... " + isPalindrome(testCase[k]));
      System.out.println(new String(new char[100]).replace('\0', '-'));
    }
  }
}
```
{% endcode %}

## **Just the Code**

Here’s the complete solution to this problem:

<pre class="language-java" data-title="ValidPalindrome3.java" data-overflow="wrap" data-line-numbers data-full-width="true"><code class="lang-java"><strong>class ValidPalindrome {
</strong>
  public static boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    while (left &#x3C; right) {
      if (s.charAt(left) != s.charAt(right)) {
        return false;
      }
      left = left + 1;
      right = right - 1;
    }
    return true;
  }
  
  //Driver code
  public static void main(String[] arg) {
    String[] testCase = {
      "RACEACAR",
      "A",
      "ABCDEFGFEDCBA",
      "ABC",
      "ABCBA",
      "ABBA",
      "RACEACAR"
    };
    for (int k = 0; k &#x3C; testCase.length; k++) {
      System.out.println("Test Case #" + (k + 1));
      System.out.println(new String(new char[100]).replace('\0', '-'));
      System.out.println("The input string is " + testCase[k] + "' and the length of the string is " + testCase[k].length() + ".");
      System.out.println("\nIs it a palindrome?..... " + isPalindrome(testCase[k]));
      System.out.println(new String(new char[100]).replace('\0', '-'));
    }
  }
}
</code></pre>

## **Summary**

* Initialize two pointers and move them from opposite ends.
* The first pointer starts at the beginning of the string and moves toward the middle, while the second pointer starts at the end and moves toward the middle.
* Compare the elements at each position to detect a non-matching pair.
* If both pointers reach the middle of the string without encountering a non-matching pair, the string is a palindrome.

### **Time Complexity** $$O(n)$$

The time complexity is $$O(n)$$, where $$n$$ is the number of characters in the string. However, the algorithm will only run $$(n/2)$$ times, since two pointers are traversing toward each other.

### **Space Complexity** $$O(1)$$

The space complexity is $$O(1)$$, since constant space is used to store two indexes.
