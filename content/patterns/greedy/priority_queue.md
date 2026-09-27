---
title: "Priority Queue / Heap Pattern"
---

## Core Idea

When the **locally optimal choice changes dynamically** as the algorithm processes data, a static sort is not enough.

You need a data structure that continuously extracts the global maximum or minimum efficiently.

## Sub Patterns

- **Huffman Coding / Rope Cutting:** Continually merge the two smallest elements to minimize overall cost.
- **Task Scheduler / Cooling Time:** Pick the available task with the highest remaining frequency.
- **Graph Traversal:** Dijkstra's Algorithm and Prim's Algorithm use a Min-Heap to greedily select the closest unvisited node.
- **Resource Allocation:** Continuously choose the resource or task that gives the best current outcome.

## Standard Python Template — Connect Ropes with Minimum Cost

```python
import heapq

def min_cost_to_connect_ropes(ropes):
    # Step 1: Transform list into a Min-Heap
    heapq.heapify(ropes)
    total_cost = 0

    # Step 2: Greedily combine the two smallest elements
    while len(ropes) > 1:
        first = heapq.heappop(ropes)
        second = heapq.heappop(ropes)

        current_cost = first + second
        total_cost += current_cost

        # Push the combined result back into the heap
        heapq.heappush(ropes, current_cost)

    return total_cost
```

## Common Problems

| Problem | Core Greedy Idea |
|---|---|
| [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/) | Repeatedly process the two largest stones using a Max-Heap. |
| [IPO](https://leetcode.com/problems/ipo/) | Among affordable projects, repeatedly choose the one with maximum profit. |
| [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/) | Use a Min-Heap to reserve ladders for the largest climbs. |
| [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | Schedule the most frequent remaining tasks while respecting cooldown periods. |
| [Minimum Cost to Connect Sticks](https://leetcode.com/problems/minimum-cost-to-connect-sticks/) | Repeatedly combine the two smallest sticks. |
| [Reorganize String](https://leetcode.com/problems/reorganize-string/) | Repeatedly select the most frequent available character while avoiding adjacent duplicates. |
