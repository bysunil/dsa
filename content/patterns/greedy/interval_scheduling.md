---
title: "Interval / Scheduling Pattern"
---

## Core Idea

This is the most common pattern in competitive programming and interviews. You are given a set of intervals with `start` and `end` times and need to maximize or minimize a specific metric.

## Sub Patterns

- **Activity Selection:** Maximize the number of tasks you can complete.
  - Greedy choice: **Sort by earliest end time**.
- **Interval Merging / Insertion:** Combine overlapping events.
  - Greedy choice: **Sort by start time**.
- **Minimum Meeting Rooms:** Find the minimum resources needed to host all events.
  - Greedy choice: **Sort by start time and track end times with a Min-Heap**.

## Standard Python Template — Activity Selection

```python
def max_activities(intervals):
    # Step 1: Sort intervals by their END times
    intervals.sort(key=lambda x: x[1])

    count = 0
    last_end_time = float("-inf")
    result = []

    # Step 2: Iterate and greedily pick the next compatible interval
    for start, end in intervals:
        if start >= last_end_time:
            result.append((start, end))
            last_end_time = end
            count += 1

    return count, result
```

## Common Problems

| Problem | Core Greedy Idea |
|---|---|
| [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | Sort by end time and keep the maximum number of non-overlapping intervals. |
| [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) | Sort by end coordinate and shoot an arrow at the earliest possible end. |
| [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | Sort by start time and merge overlapping intervals. |
| [Insert Interval](https://leetcode.com/problems/insert-interval/) | Merge the new interval with all overlapping intervals. |
| [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) | Sort intervals by start time and check for overlaps. |
| [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | Sort by start time and use a Min-Heap of ending times. |
| [Partition Labels](https://leetcode.com/problems/partition-labels/) | Track the last occurrence of each character to form maximal partitions. |
