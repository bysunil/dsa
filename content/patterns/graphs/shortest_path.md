---
title: "Shortest Path Algorithms"
---

## 1. Dijkstra's Algorithm

### When to Use

- Finding single-source shortest paths on non-negative weighted graphs.
- Works on both node-based graphs (adjacency lists) and grid-based graphs.

### Edge Cases

- Negative weight edges (Standard Dijkstra will fail, use Bellman-Ford).
- Unreachable nodes (distance remains infinity).

### Node-Based Template

```python
import heapq

def dijkstra(
    graph: dict[int, list[tuple[int, float]]],
    start: int,
    num_nodes: int
) -> dict[int, float]:
    distances = {i: float("inf") for i in range(num_nodes)}
    distances[start] = 0
    pq = [(0, start)]  # (distance, node)

    while pq:
        current_dist, current_node = heapq.heappop(pq)

        # Skip stale entries
        if current_dist > distances[current_node]:
            continue

        for neighbor, weight in graph.get(current_node, []):
            new_dist = current_dist + weight

            if new_dist < distances[neighbor]:
                distances[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))

    return distances
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Network Delay Time](https://leetcode.com/problems/network-delay-time/) | Find the longest of all shortest paths from the source using Dijkstra's algorithm. |
| [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/) | Use Dijkstra but with a max-heap and multiply probabilities instead of adding weights. |

### Grid-Based Template

```python
import heapq

def dijkstra_grid(grid: list[list[int]], start_r: int, start_c: int):
    rows, cols = len(grid), len(grid[0])
    distances = [[float('inf')] * cols for _ in range(rows)]
    distances[start_r][start_c] = grid[start_r][start_c]

    pq = [(grid[start_r][start_c], start_r, start_c)]  # (cost, row, col)

    while pq:
        current_cost, r, c = heapq.heappop(pq)

        if current_cost > distances[r][c]:
            continue

        for dr, dc in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols:
                next_cost = current_cost + grid[nr][nc]
                if next_cost < distances[nr][nc]:
                    distances[nr][nc] = next_cost
                    heapq.heappush(pq, (next_cost, nr, nc))
    return distances
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/) | Use Dijkstra's algorithm to find a path that minimizes the maximum absolute difference in heights. |
| [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/) | Find the path that minimizes the maximum elevation along the way using Dijkstra with a priority queue. |

## 2. Bellman-Ford Algorithm

Used for finding shortest paths from a single source when the graph contains negative weight edges. It can also detect negative-weight cycles.

```python
def bellman_ford(num_nodes: int, edges: list[tuple[int, int, float]], start: int) -> list[float] | None:
    distances = [float("inf")] * num_nodes
    distances[start] = 0

    for _ in range(num_nodes - 1):
        updated = False
        for u, v, weight in edges:
            if distances[u] != float("inf") and distances[u] + weight < distances[v]:
                distances[v] = distances[u] + weight
                updated = True
        if not updated:
            break

    for u, v, weight in edges:
        if distances[u] != float("inf") and distances[u] + weight < distances[v]:
            return None  # Negative cycle

    return distances
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) | Use Bellman-Ford for exactly K iterations. |

## 3. Floyd-Warshall Algorithm

Used to find the shortest paths between all pairs of nodes in a weighted graph. It handles negative weights and can detect negative cycles.
Input `edges` should be a list of `(u, v, weight)` representing a directed edge `u -> v`.

```python
def floyd_warshall(
    num_nodes: int,
    edges: list[tuple[int, int, float]]
) -> list[list[float]] | None:
    dist = [[float("inf")] * num_nodes for _ in range(num_nodes)]

    for i in range(num_nodes):
        dist[i][i] = 0

    for u, v, weight in edges:
        dist[u][v] = min(dist[u][v], weight)

    for k in range(num_nodes):
        for i in range(num_nodes):
            for j in range(num_nodes):
                if (
                    dist[i][k] != float("inf")
                    and dist[k][j] != float("inf")
                ):
                    dist[i][j] = min(
                        dist[i][j],
                        dist[i][k] + dist[k][j]
                    )

    # Check for negative-weight cycles
    for i in range(num_nodes):
        if dist[i][i] < 0:
            return None

    return dist
```

#### Common Problems

| Problem | Core Idea |
|---|---|
| [Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/) | Run Floyd-Warshall to compute all-pairs shortest paths, then count reachable neighbors. |
