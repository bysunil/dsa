---
title: "Bipartite Graph"
---

## What is a Bipartite Graph?

A bipartite graph is a graph whose vertices can be divided into two disjoint sets $U$ and $V$ such that every edge connects a vertex in $U$ to one in $V$. No two nodes in the same set are connected.

- Can be checked using graph coloring (2 colors).
- A graph is bipartite if and only if it does not contain an odd-length cycle.

## Graph Coloring Template (BFS)

```python
from collections import deque

def is_bipartite(graph: list[list[int]]) -> bool:
    n = len(graph)
    colors = [-1] * n  # -1 means uncolored, 0 and 1 are the two colors

    for i in range(n):
        if colors[i] != -1:
            continue
            
        queue = deque([i])
        colors[i] = 0

        while queue:
            node = queue.popleft()

            for neighbor in graph[node]:
                if colors[neighbor] == -1:
                    colors[neighbor] = 1 - colors[node]
                    queue.append(neighbor)
                elif colors[neighbor] == colors[node]:
                    return False  # Conflict found

    return True
```

## Graph Coloring Template (DFS)

```python
def is_bipartite_dfs(graph: list[list[int]]) -> bool:
    n = len(graph)
    colors = [-1] * n

    def dfs(node: int, color: int) -> bool:
        colors[node] = color
        for neighbor in graph[node]:
            if colors[neighbor] == -1:
                if not dfs(neighbor, 1 - color):
                    return False
            elif colors[neighbor] == colors[node]:
                return False
        return True

    for i in range(n):
        if colors[i] == -1:
            if not dfs(i, 0):
                return False

    return True
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/) | Basic bipartite graph check using BFS/DFS. |
| [Possible Bipartition](https://leetcode.com/problems/possible-bipartition/) | Group people into two sets given dislike pairs. Convert to graph and check if bipartite. |
