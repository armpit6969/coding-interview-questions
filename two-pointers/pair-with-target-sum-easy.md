---
description: Try to solve the 'Pair with Target Sum' problem.
---

# Pair with Target Sum (easy)

## Problem Statement&#x20;

Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target. Write a function to return the indices of the two numbers (i.e. the pair) such that they add up to the given target.

## Examples

_Input_: \[1, 2, 3, 4, 6], target=6 \
_Output_: \[1, 3] \
_Explanation_: The numbers at index 1 and 3 add up to 6: 2 + 4 = 6

_Input_: \[2, 5, 9, 11], target=11\
_Output_: \[0, 2]\
_Explanation_: The numbers at index 0 and 2 add up to 11: 2 + 9 = 11

## Solution

Since the given array is sorted, a brute-force solution could be to iterate through the array, taking one number at a time and searching for the second number through Binary Search. The time complexity of this algorithm will be O(N \* log(N)). Can we do better than this?

We can follow the Two Pointers approach. We will start with one pointer pointing to the beginning of the array and another pointing at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do, we have found our pair; otherwise, we will do one of two things:

1. If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

```java
class PairWithTargetSum {

  public static int[] search(int[] arr, int targetSum) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
      int currentSum = arr[left] + arr[right];
      if (currentSum == targetSum)
        return new int[] { left, right }; // found the pair

      if (targetSum > currentSum)
        left++; // we need a pair with a bigger sum
      else
        right--; // we need a pair with a smaller sum
    }
    return new int[] { -1, -1 };
  }

  public static void main(String[] args) {
    int[] result = PairWithTargetSum.search(new int[] { 1, 2, 3, 4, 6 }, 6);
    System.out.println("Pair with target sum: [" + result[0] + ", " + result[1] + "]");
    result = PairWithTargetSum.search(new int[] { 2, 5, 9, 11 }, 11);
    System.out.println("Pair with target sum: [" + result[0] + ", " + result[1] + "]");
  }
}
```

## Summary

### **Time Complexity** $$O(n)$$

The time complexity of the above algorithm will be $$O(n)$$, where $$n$$ is the total number of elements in the given array.

### **Space Complexity** $$O(1)$$

### **An Alternate Approach**&#x20;

Instead of using a two-pointer or a binary search approach, we can utilize a HashTable to search for the required pair. We can iterate through the array one number at a time. Let’s say during our iteration we are at number ‘X’, so we need to find ‘Y’ such that “X + Y == Target”. We will do two things here:

1. Search for ‘Y’ (which is equivalent to “Target - X”) in the HashTable. If it is there, we have found the required pair. Otherwise, insert “X” in the HashTable, so that we can search it for the later numbers.
2. Here is what our algorithm will look like:

```java
import java.util.HashMap;

class PairWithTargetSum {

  public static int[] search(int[] arr, int targetSum) {
    HashMap<Integer, Integer> nums = new HashMap<>(); // to store numbers and their indices
    for (int i = 0; i < arr.length; i++) {
      if (nums.containsKey(targetSum - arr[i]))
        return new int[] { nums.get(targetSum - arr[i]), i };
      else
        nums.put(arr[i], i); // put the number and its index in the map
    }
    return new int[] { -1, -1 }; // pair not found
  }

  public static void main(String[] args) {
    int[] result = PairWithTargetSum.search(new int[] { 1, 2, 3, 4, 6 }, 6);
    System.out.println("Pair with target sum: [" + result[0] + ", " + result[1] + "]");
    result = PairWithTargetSum.search(new int[] { 2, 5, 9, 11 }, 11);
    System.out.println("Pair with target sum: [" + result[0] + ", " + result[1] + "]");
  }
}
```
