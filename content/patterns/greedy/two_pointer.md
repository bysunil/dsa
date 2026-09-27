---
title: "Two-Pointer / Two-End Pattern"
---

## Core Idea

This pattern applies when your input array is sorted, and you can achieve a goal by comparing elements from opposite ends (or staggered positions) of the array to make a locally optimal choice.

## Sub Patterns

- **Boat / Container Packing (e.g., LeetCode 881):** Minimize the number of boats to carry people under a weight limit.
  - Greedy choice: **Pair the heaviest person with the lightest person**.
- **Container With Most Water:** Move pointers inward based on whichever boundary is shorter.
- **Pairing / Matching:** Sort both sides and greedily match the smallest compatible elements.

## Standard Python Template — Boats to Save People

```python
def num_rescue_boats(people, limit):
    # Step 1: Sort the weights
    people.sort()

    left = 0
    right = len(people) - 1
    boats = 0

    # Step 2: Greedily pair opposite extremes
    while left <= right:
        if left == right:
            # Only one person left
            boats += 1
            break

        # If heaviest + lightest fit, they share a boat
        if people[left] + people[right] <= limit:
            left += 1

        # Heaviest always gets a boat
        # (either alone or with the lightest)
        right -= 1
        boats += 1

    return boats
```

## Common Problems

| Problem | Core Greedy Idea |
|---|---|
| [Boats to Save People](https://leetcode.com/problems/boats-to-save-people/) | Pair the heaviest person with the lightest person whenever possible. |
| [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | Move the pointer at the shorter boundary because the shorter side limits the area. |
| [Assign Cookies](https://leetcode.com/problems/assign-cookies/) | Sort children and cookies, then give each child the smallest cookie that satisfies them. |
| [Bag of Tokens](https://leetcode.com/problems/bag-of-tokens/) | Use the smallest token to gain score and the largest token to regain power when needed. |
| [Two City Scheduling](https://leetcode.com/problems/two-city-scheduling/) | Sort people by the difference in cost between the two cities and assign greedily. |
