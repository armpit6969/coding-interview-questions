---
description: Try to solve the 'Total Cost to Hire K Workers' problem.
---

# Total Cost to Hire K Workers (medium)

## Problem Statement

You are given a **0-indexed** integer array `costs` where `costs[i]` is the cost of hiring the `ith` worker.

You are also given two integers `k` and `candidates`. We want to hire exactly `k` workers according to the following rules:

* You will run `k` sessions and hire exactly one worker in each session.
* In each hiring session, choose the worker with the lowest cost from either the first `candidates` workers or the last `candidates` workers. Break the tie by the smallest index.
  * For example, if `costs = [3,2,7,7,1,2]` and `candidates = 2`, then in the first hiring session, we will choose the `4th` worker because they have the lowest cost `[3,2,7,7,`**`1`**`,2]`.
  * In the second hiring session, we will choose `1st` worker because they have the same lowest cost as `4th` worker but they have the smallest index `[3,`**`2`**`,7,7,2]`. Please note that the indexing may be changed in the process.
* If there are fewer than candidates workers remaining, choose the worker with the lowest cost among them. Break the tie by the smallest index.
* A worker can only be chosen once.

Return _the total cost to hire exactly_ `k` _workers._

## Examples

**Example 1:**

<pre data-full-width="true"><code><strong>Input: costs = [17,12,10,2,7,2,11,20,8], k = 3, candidates = 4
</strong><strong>Output: 11
</strong><strong>Explanation: We hire 3 workers in total. The total cost is initially 0.
</strong>- In the first hiring round we choose the worker from [17,12,10,2,7,2,11,20,8]. The lowest cost is 2, and we break the tie by the smallest index, which is 3. The total cost = 0 + 2 = 2.
- In the second hiring round we choose the worker from [17,12,10,7,2,11,20,8]. The lowest cost is 2 (index 4). The total cost = 2 + 2 = 4.
- In the third hiring round we choose the worker from [17,12,10,7,11,20,8]. The lowest cost is 7 (index 3). The total cost = 4 + 7 = 11. Notice that the worker with index 3 was common in the first and last four workers.
The total hiring cost is 11.
</code></pre>

**Example 2:**

<pre data-full-width="true"><code><strong>Input: costs = [1,2,4,1], k = 3, candidates = 3
</strong><strong>Output: 4
</strong><strong>Explanation: We hire 3 workers in total. The total cost is initially 0.
</strong>- In the first hiring round we choose the worker from [1,2,4,1]. The lowest cost is 1, and we break the tie by the smallest index, which is 0. The total cost = 0 + 1 = 1. Notice that workers with index 1 and 2 are common in the first and last 3 workers.
- In the second hiring round we choose the worker from [2,4,1]. The lowest cost is 1 (index 2). The total cost = 1 + 1 = 2.
- In the third hiring round there are less than three candidates. We choose the worker from the remaining workers [2,4]. The lowest cost is 2 (index 0). The total cost = 2 + 2 = 4.
The total hiring cost is 4.
</code></pre>

## **Constraints**

* `1 <= costs.length <= 105`
* `1 <= costs[i] <= 105`
* `1 <= k, candidates <= costs.length`

## Solution

### **Intuition** _"Two Priority Queues"_

> If you are not familiar with the priority queue, please refer to our explore cards [Heaps Explore Card](https://leetcode.com/explore/featured/card/graph/619/depth-first-search-in-graph/). We will focus on the usage in this article and not the implementation details.

**For the sake of brevity, let `m` represent the input integer `candidates` for the rest of the article.**

To begin with, we need to understand the problem requirements. In each of the `k` hiring rounds, we must hire a worker with the lowest cost (with the smallest index being a tiebreaker) based on the provided rules.

We have the option to select the worker with the lowest cost from either the first `m` candidates or the last `m` candidates from `costs`. Once we choose a worker from either of these sections, we remove the chosen worker from the array, which makes space for another worker to be in either the first or last `m` candidates. We continue to select the worker with the lowest cost, each time making space for another worker from `costs` to be into consideration. Because we need to repeatedly find the minimum cost, using a priority queue is the most appropriate approach to simulate this process.

During each hiring session, our goal is to select the worker with the lowest cost. As mentioned above, after selecting a worker, a spot will open up for another worker to be among the first or last `m` candidates. As such, we need to distinguish between the first `m` candidates and the last `m` candidates. That way, when we choose a worker, we know if a spot was opened in the first `m` candidates or the last `m` candidates.

![Create two priority queues: the first m candidates as head\_workers and the last m candidates as tail\_workers](https://leetcode.com/problems/total-cost-to-hire-k-workers/Figures/2462/1.png)

To store the workers in two sections separately, we can use two priority queues, `head_workers` and `tail_workers`, where the worker with the lowest cost has the highest priority.

![The worker with the lowest cost has the highest priority](https://leetcode.com/problems/total-cost-to-hire-k-workers/Figures/2462/2.png)

Throughout the process, after we hire a worker from a section, we need to add an additional candidate to this section. Therefore, we need two pointers, `next_head` and `next_tail`, that denotes the next worker to be added to the respective queues.

![Set a head/tail pointer to the next set of workers](https://leetcode.com/problems/total-cost-to-hire-k-workers/Figures/2462/3.png)

Just like in this situation shown in the picture, if two workers with the same cost appear at the top of both queues, we will hire the one from `head_workers`, since this worker has a smaller index compared with the other one from `tail_workers`. Afterwards, we need to refill `head_workers` with the worker at `next_head` to ensure that it still contains the first `m` unselected candidates.

![Add the worker with smallest cost to the appropriate heap](https://leetcode.com/problems/total-cost-to-hire-k-workers/Figures/2462/4.png)

We add the worker `costs[next_head]` to `head_workers`, and then increment this pointer by 1, indicating the next unselected worker.

![img](https://leetcode.com/problems/total-cost-to-hire-k-workers/Figures/2462/5.png)

However, if we encounter the condition `next_tail < next_head`, it indicates that all the workers have been selected as candidates and there are no more workers outside the two queues. To avoid double counting, we should not add a worker to both queues or update either pointer. Therefore, we can simply move on without making any updates to the queues or pointers.

![img](https://leetcode.com/problems/total-cost-to-hire-k-workers/Figures/2462/6.png)

### **Algorithm**

1. Initialize two priority queues `head_workers` and `tail_workers` that store the first `m` workers and the last `m` workers, where the worker with the lowest cost has the highest priority.
2. Set up two pointers `next_head = m`, `next_tail = n - m - 1` indicating the next worker to be added to two queues.
3. Compare the top workers in both queues, and hire the one with the lowest cost, if both workers have the same cost, hire the worker from `head_workers`. Add the cost of this worker to the total cost.
4.  If `next_head <= next_tail`, we need to fill the queue with one worker:

    * If the hired worker is from `head_workers`, we add the worker `costs[next_head]` to it and increment `next_head` by 1.
    * If the hired worker is from `tail_workers`, we add the worker `costs[tail_head]` to it and decrement `tail_head` by 1.

    Otherwise, skip this step.
5. Repeat steps 3 and 4 `k` times.
6. Return the total cost of all the hired workers.

## Just the Code

{% code title="Solution.java" overflow="wrap" lineNumbers="true" fullWidth="true" %}
```java
class Solution {
    public long totalCost(int[] costs, int k, int candidates) {
        PriorityQueue<Integer> headWorkers = new PriorityQueue<>();
        PriorityQueue<Integer> tailWorkers = new PriorityQueue<>();
        
        // headWorkers stores the first k workers.
        // tailWorkers stores at most last k workers without any workers from the first k workers.
        for (int i = 0; i < candidates; i++) {
            headWorkers.add(costs[i]);
        }
        for (int i = Math.max(candidates, costs.length - candidates); i < costs.length; i++) {
            tailWorkers.add(costs[i]);
        }

        long answer = 0;
        int nextHead = candidates;
        int nextTail = costs.length - 1 - candidates;

        for (int i = 0; i < k; i++) {
            if (tailWorkers.isEmpty() || !headWorkers.isEmpty() && headWorkers.peek() <= tailWorkers.peek()) {
                answer += headWorkers.poll();
                
                // Only refill the queue if there are workers outside the two queues.
                if (nextHead <= nextTail) {
                    headWorkers.add(costs[nextHead]);
                    nextHead++;
                }
            } 
            
            else {
                answer += tailWorkers.poll();

                // Only refill the queue if there are workers outside the two queues.
                if (nextHead <= nextTail) {
                    tailWorkers.add(costs[nextTail]);
                    nextTail--;
                }
            }
        }

        return answer;
    }
}
```
{% endcode %}

## Summary

Let $$m$$ be the given integer `candidates`.

### Time Complexity $$O((k+m)⋅log⁡m)$$

* We need to initialize two priority queues of size $$mmm$$, which takes $$O(m⋅log⁡m)$$ time.
* During the hiring rounds, we keep removing the top element from priority queues and adding new elements for up to $$k$$ times. Operations on a priority queue take amortized time. Thus this process takes $$O(k⋅log⁡m)$$ time.
* Note: in Python, `heapq.heapify()` creates the priority queue in linear time. Therefore, in Python, the time complexity is $$O(m+k⋅log⁡m)$$.

### Space Complexity $$O(m)$$

* We need to store the first $$m$$ and the last $$m$$ workers in two priority queues.
