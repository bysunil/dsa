---
title: "Advanced Graph Algorithms"
---

## 1. Kosaraju's Algorithm (SCCs)

Used to find Strongly Connected Components (SCCs) in a directed graph using a two-pass DFS approach.
Input `edges` should be a list of pairs `(u, v)` representing a directed edge `u -> v`.

```python
from collections import defaultdict

def kosaraju_scc(
    num_nodes: int,
    edges: list[tuple[int, int]]
) -> list[list[int]]:
    graph = defaultdict(list)
    rev_graph = defaultdict(list)

    for u, v in edges:
        graph[u].append(v)
        rev_graph[v].append(u)

    visited = set()
    order = []

    def dfs1(node: int) -> None:
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs1(neighbor)
        order.append(node)

    for node in range(num_nodes):
        if node not in visited:
            dfs1(node)

    visited.clear()
    sccs = []

    def dfs2(node: int, component: list[int]) -> None:
        visited.add(node)
        component.append(node)
        for neighbor in rev_graph[node]:
            if neighbor not in visited:
                dfs2(neighbor, component)

    for node in reversed(order):
        if node not in visited:
            component = []
            dfs2(node, component)
            sccs.append(component)

    return sccs
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Maximum Number of Strongly Connected Components](https://www.geeksforgeeks.org/strongly-connected-components/) | Find SCCs using Kosaraju's two-pass algorithm. |

## 2. Tarjan's Algorithm (SCCs)

Used to find Strongly Connected Components (SCCs) and bridges in a directed graph in a single DFS pass using discovery times and low-link values.
Input `edges` should be a list of pairs `(u, v)` representing a directed edge `u -> v`.

```python
from collections import defaultdict

def tarjan_scc(
    num_nodes: int,
    edges: list[tuple[int, int]]
) -> list[list[int]]:
    graph = defaultdict(list)

    for u, v in edges:
        graph[u].append(v)

    discovery_time = [-1] * num_nodes
    low = [-1] * num_nodes
    on_stack = [False] * num_nodes
    stack = []
    sccs = []
    timer = 0

    def dfs(u: int) -> None:
        nonlocal timer

        discovery_time[u] = low[u] = timer
        timer += 1

        stack.append(u)
        on_stack[u] = True

        for v in graph[u]:
            if discovery_time[v] == -1:
                dfs(v)
                low[u] = min(low[u], low[v])

            elif on_stack[v]:
                # Back-edge to an ancestor on the stack
                low[u] = min(low[u], discovery_time[v])

        # If u is the root of an SCC
        if low[u] == discovery_time[u]:
            component = []

            while True:
                w = stack.pop()
                on_stack[w] = False
                component.append(w)

                if w == u:
                    break

            sccs.append(component)

    for i in range(num_nodes):
        if discovery_time[i] == -1:
            dfs(i)

    return sccs
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Critical Connections in a Network](https://leetcode.com/problems/critical-connections-in-a-network/) | Find bridges in an undirected graph using Tarjan's discovery time and low-link values. |

## 3. Tarjan's Algorithm (Articulation Points)

Used to find articulation points (cut vertices) in an undirected graph. Removing an articulation point disconnects the graph.

```python
from collections import defaultdict

def tarjan_articulation_points(
    num_nodes: int,
    edges: list[tuple[int, int]]
) -> list[int]:
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    discovery_time = [-1] * num_nodes
    low = [-1] * num_nodes
    timer = 0
    articulation_points = set()

    def dfs(u: int, p: int = -1) -> None:
        nonlocal timer
        discovery_time[u] = low[u] = timer
        timer += 1
        children = 0

        for v in graph[u]:
            if v == p:
                continue
            
            if discovery_time[v] != -1:
                # Back-edge
                low[u] = min(low[u], discovery_time[v])
            else:
                # Tree edge
                children += 1
                dfs(v, u)
                low[u] = min(low[u], low[v])
                
                # Condition for articulation point
                if p != -1 and low[v] >= discovery_time[u]:
                    articulation_points.add(u)

        # Root condition
        if p == -1 and children > 1:
            articulation_points.add(u)

    for i in range(num_nodes):
        if discovery_time[i] == -1:
            dfs(i)

    return list(articulation_points)
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Articulation Point - I (GFG)](https://www.geeksforgeeks.org/problems/articulation-point-1/1) | Use Tarjan's algorithm to find cut vertices. |
