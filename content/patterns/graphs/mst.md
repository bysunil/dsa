---
title: "Minimum Spanning Tree (MST)"
---

## What is a Minimum Spanning Tree?

A Minimum Spanning Tree (MST) of a weighted, connected, undirected graph is a spanning tree with a weight less than or equal to the weight of every other spanning tree.
- **Spanning Tree:** A subset of edges that connects all vertices without any cycles.
- **Applications:** Network design (laying cables, building roads), clustering.

---

## 1. Kruskal's Algorithm

Kruskal's algorithm finds the MST by sorting edges based on their weights and adding them one by one if they don't form a cycle. It uses a Disjoint Set Union (DSU) data structure to efficiently detect cycles.

### Template

```python
class DSU:
    def __init__(self, n: int):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, i: int) -> int:
        if self.parent[i] == i:
            return i
        self.parent[i] = self.find(self.parent[i])
        return self.parent[i]

    def union(self, i: int, j: int) -> bool:
        root_i = self.find(i)
        root_j = self.find(j)
        if root_i == root_j:
            return False  # Cycle detected
        
        if self.rank[root_i] < self.rank[root_j]:
            self.parent[root_i] = root_j
        elif self.rank[root_i] > self.rank[root_j]:
            self.parent[root_j] = root_i
        else:
            self.parent[root_j] = root_i
            self.rank[root_i] += 1
        return True

def kruskal_mst(num_nodes: int, edges: list[tuple[int, int, int]]) -> int:
    """
    edges: list of (u, v, weight)
    Returns the total weight of the MST or -1 if the graph is disconnected.
    """
    dsu = DSU(num_nodes)
    edges.sort(key=lambda x: x[2])  # Sort by weight
    
    mst_weight = 0
    edges_used = 0
    
    for u, v, weight in edges:
        if dsu.union(u, v):
            mst_weight += weight
            edges_used += 1
            if edges_used == num_nodes - 1:
                break
                
    return mst_weight if edges_used == num_nodes - 1 else -1
```

---

## 2. Prim's Algorithm

Prim's algorithm builds the MST starting from a single node and eagerly adding the lowest-weight edge that connects a node in the tree to a node outside the tree. It uses a Priority Queue (Min-Heap).

### Template

```python
import heapq
from collections import defaultdict

def prim_mst(num_nodes: int, edges: list[tuple[int, int, int]]) -> int:
    """
    edges: list of (u, v, weight)
    Returns the total weight of the MST or -1 if the graph is disconnected.
    """
    graph = defaultdict(list)
    for u, v, w in edges:
        graph[u].append((v, w))
        graph[v].append((u, w))
        
    # Min-heap stores (weight, node)
    pq = [(0, 0)]  # Start from node 0 with weight 0
    visited = set()
    mst_weight = 0
    
    while pq and len(visited) < num_nodes:
        weight, node = heapq.heappop(pq)
        
        if node in visited:
            continue
            
        visited.add(node)
        mst_weight += weight
        
        for neighbor, edge_weight in graph[node]:
            if neighbor not in visited:
                heapq.heappush(pq, (edge_weight, neighbor))
                
    return mst_weight if len(visited) == num_nodes else -1
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/) | Find the MST weight given coordinates. Both Kruskal's and Prim's work perfectly. |
| [Find Critical and Pseudo-Critical Edges in MST](https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/) | Use Kruskal's algorithm to analyze the impact of enforcing or ignoring specific edges. |
