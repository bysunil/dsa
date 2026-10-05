---
title: "Cycle Detection"
---

Cycle detection is a fundamental operation in graphs. The approach differs based on whether the graph is **undirected** or **directed**.

---

## 1. Undirected Graph

In an undirected graph, a cycle exists if during a traversal (DFS/BFS) we reach a previously visited node that is **not the immediate parent** of the current node. Alternatively, **Disjoint Set Union (DSU)** can be used.

### Approach 1: DFS

```python
from collections import defaultdict

def has_cycle_undirected_dfs(num_nodes: int, edges: list[tuple[int, int]]) -> bool:
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
        
    visited = set()
    
    def dfs(node: int, parent: int) -> bool:
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                if dfs(neighbor, node):
                    return True
            elif neighbor != parent:
                return True
        return False

    for i in range(num_nodes):
        if i not in visited:
            if dfs(i, -1):
                return True
                
    return False
```

### Approach 2: Union-Find (DSU)

See the [Disjoint Set Union](./dsu.md) pattern for the full DSU template. DSU is excellent for checking cycles as you iteratively build the graph.

```python
# Assuming a basic DSU implementation exists
def has_cycle_undirected_dsu(num_nodes: int, edges: list[tuple[int, int]]) -> bool:
    dsu = DSU(num_nodes)
    for u, v in edges:
        if not dsu.union(u, v):
            return True  # A cycle is formed
    return False
```

---

## 2. Directed Graph

In a directed graph, a cycle exists if we encounter a node that is currently in our **recursion stack** (or being actively visited). Just checking `visited` is not enough.

### Approach 1: DFS (3-Color / Recursion Stack)

We can maintain the state of each node:
- `0`: Unvisited
- `1`: Visiting (currently in the recursion stack)
- `2`: Visited (fully processed)

If we encounter a node with state `1` during DFS, we've found a cycle.

```python
from collections import defaultdict

def has_cycle_directed_dfs(num_nodes: int, edges: list[tuple[int, int]]) -> bool:
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        
    state = [0] * num_nodes  # 0=Unvisited, 1=Visiting, 2=Visited
    
    def dfs(node: int) -> bool:
        if state[node] == 1:
            return True  # Found a back-edge (cycle)
        if state[node] == 2:
            return False # Already processed
            
        state[node] = 1
        for neighbor in graph[node]:
            if dfs(neighbor):
                return True
        state[node] = 2
        
        return False
        
    for i in range(num_nodes):
        if state[i] == 0:
            if dfs(i):
                return True
                
    return False
```

### Approach 2: Kahn's Algorithm (BFS)

Kahn's Algorithm is traditionally used for Topological Sorting. However, if a topological sort cannot process all vertices (i.e., the count of processed vertices is less than the total vertices), the graph contains a cycle.

See the [Kahn's Algorithm](./kahns_algorithm.md) pattern for the template.

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Redundant Connection](https://leetcode.com/problems/redundant-connection/) | Undirected cycle detection using DSU or DFS. |
| [Course Schedule](https://leetcode.com/problems/course-schedule/) | Directed cycle detection using 3-color DFS or Kahn's Algorithm. |
