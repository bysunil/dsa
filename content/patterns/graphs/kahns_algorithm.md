---
title: "Kahn's Algorithm (Topological Sort)"
---

## What is Kahn's Algorithm?

Used to find a topological ordering of a Directed Acyclic Graph (DAG) using a queue to process nodes with an in-degree of 0.

## Template

```python
from collections import deque

def topological_sort_kahn(num_nodes: int, edges: list[tuple[int, int]]) -> list[int]:
    graph = {i: [] for i in range(num_nodes)}
    in_degree = [0] * num_nodes

    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1

    queue = deque(i for i in range(num_nodes) if in_degree[i] == 0)
    topo_order = []

    while queue:
        node = queue.popleft()
        topo_order.append(node)

        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return topo_order if len(topo_order) == num_nodes else []
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Course Schedule](https://leetcode.com/problems/course-schedule/) | Detect if a cycle exists in a prerequisite graph. |
