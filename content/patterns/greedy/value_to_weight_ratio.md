---
title: "Value-to-Weight Ratio Pattern"
---

## Core Idea

Used when items have multiple dimensions—usually a cost/benefit and a size/weight—and you have a constrained budget or capacity.

## Sub Patterns

- **Fractional Knapsack:** Maximize total value by taking fractions of items.
  - Greedy choice: **Sort by value density, i.e., Value / Weight**.
- **Gas Station Problem:** Calculate if you can complete a circuit based on the net difference of gas vs. distance.
- **Profit / Cost Prioritization:** Select items or opportunities according to the best immediate benefit relative to the available constraint.

## Standard Python Template — Fractional Knapsack

```python
def fractional_knapsack(capacity, items):
    # items format: [(value, weight), ...]

    # Step 1: Sort by value/weight ratio in descending order
    items.sort(key=lambda x: x[0] / x[1], reverse=True)

    total_value = 0.0

    # Step 2: Take as much of the densest item as possible
    for value, weight in items:
        if capacity >= weight:
            capacity -= weight
            total_value += value
        else:
            # Take the fractional part of the remaining item
            total_value += value * (capacity / weight)
            break  # Knapsack is completely full

    return total_value
```

## Common Problems

| Problem | Core Greedy Idea |
|---|---|
| [Fractional Knapsack](https://www.geeksforgeeks.org/problems/fractional-knapsack-1587115620/1) | Sort items by value/weight ratio and take the highest-density items first. |
| [Gas Station](https://leetcode.com/problems/gas-station/) | If the total gas is sufficient, greedily restart the candidate start after a failed segment. |
| [Jump Game](https://leetcode.com/problems/jump-game/) | Maintain the farthest index reachable so far. |
| [Jump Game II](https://leetcode.com/problems/jump-game-ii/) | Expand the current reachable range and greedily jump when the range ends. |
| [Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/) | Use a Max-Heap to choose the largest available fuel amounts when refueling is necessary. |
| [Candy](https://leetcode.com/problems/candy/) | Make locally valid assignments while respecting both left and right neighbor constraints. |
