# Graphs

- Types: Directed and Undirected
- Both can be either weighted or unweighted.
- Representations:
  - Adjacency list `{ node -> { ...neighbors } }`
  - Adjacency matrix: `A[i][j]` contains 1 (edge weight) if nodes `i` and `j`
    are connected.
    ```
    [
        [a00, a01, ..., a0n],
        [a10, a11, ..., a1n],
        [..., ..., ..., ...],
        [an0, an1, ..., ann]
    ]
    ```

## Construction

Given: Edges of the form `(u, v)`

### Adjacency list

```python
graph = defaultdict(set)
for u, v in edges:
    graph[u].add(v)
    graph[v].add(u)  # only for undirected graph
```

### Adjacency matrix

```python
graph = [[0] * n for _ in range(n)]  # `n` is the number of vertices
for u, v in edges:
    graph[u][v] = 1
    graph[v][u] = 1  # only for undirected graph
```

## Depth First Search

**Time Complexity**: `O(n)`  
**Space Complexity**: `O(max_depth)`

### Recursive

```python
def dfs(vertex, visited, graph):
    if vertex in visited:
        return
    visited.add(vertex)
    for child in graph[vertex]:
        dfs(child, visited, graph)
```

### Iterative

```python
def dfs(start_vertex, graph):
    visited = set()
    stack = [start_vertex]
    while stack:
        vertex = stack.pop()
        for child in graph[vertex]:
            if child not in visited:
                stack.append(child)
        visited.add(vertex)
```

## Breadth First Search

```python
def bfs(start_vertex, graph):
    visited = {start_vertex}
    que = deque()
    que.append(start_vertex)

    while que:
        vertex = que.popleft()
        for child in graph[vertex]:
            if child not in visited:
                que.append(child)
```

**Time Complexity**: `O(n)`  
**Space Complexity**: `O(max_width)`

## Shortest path algorithms

### Breadth first search

Add distance of vertex measured from the source along with each vertex to the
queue. When adding a child to the queue add its distance as one more than the
current distance. _This algorithm only works for unweighted graph_.

### Dijkstra's algorithm

- It finds the shortest path from a given source vertex to every other vertex
  in the graph.
- It works for both weighted and unweighted graphs.
- Fails for graphs containing cycles with negative weight, i.e., if the graph
  contains a cycle that has a negative sum of edge weights, the algorithm will
  run into an infinite loop. This is because after each pass through the cycle
  the weight keeps decreasing leading to addition of the same nodes repeatedly
  with less distance each time (since negative numbers decrease with increase
  in magnitude).

```python
from math import inf
from queue import PriorityQueue

def shortest_path(source, graph):
    distance = defaultdict(lambda: inf)
    visited = set()
    pq = PriorityQueue()  # contains pairs (distance_from_source, vertex)
    pq.put((0, source))

    while not pq.empty():
        vertex, curr_distance = pq.get()
        if vertex in visited: continue
        visited.add(vertex)
        for child in graph[vertex]:
            if 1 + curr_distance < distance[child]:
                # new distance is less than previously recorded distance
                distance[child] = 1 + curr_distance
                pq.put((distance[child], curr))
```

**Time Complexity**: `O(n + e*log(n))`  
**Space Complexity**: `O(n)`

### Floyd-Warshall algorithm

- It finds the shortest path between every pair of vertices in the graph.
- Graph is represented as an adjacency matrix.
- It can handle graphs containing cycles (even with negative weights).
- Assumption: vertices are numbered from `0` through `n - 1`. If this is not
  true, first map the indexes `0` through `n - 1` to each of the vertex.
  This may require additional `O(n)` space.

```python
def shortest_path(graph):
    distance = [[inf] * n for _ in range(n)]  # `n` is the number of vertices

    for i in range(n):
        for j in range(n):
            distance[i][j] = graph[i][j]

    for i in range(n):
        for j in range(n):
            for k in range(n):
                if distance[i][k] != inf and distance[j][k] != inf:
                    # try to minimize distance only when both (i, k) and (k, j)
                    # have valid paths between them.
                    distance[i][j] = min(
                        distance[i][j],
                        distance[i][k] + distance[k][j]
                    )
```

**Time Complexity**: `O(n**3)`  
**Space Complexity**: `O(n**2)` (`distance` matrix)

## Disjoint Set Union

- This data structure finds the group of a node in constant time in the long
  run. This is achieved by path compression in the `find` method.

```python
class DisjointSet:
    def __init__(self, n):
        self.parent = [v for v in range(n)]
        self.size = [1 for _ in range(n)]

    def find(self, v):
        if v != self.parent[v]:
            # path compression
            self.parent[v] = self.find(self.parent[v])
        return self.parent[v]

    def union(self, u, v):
        p, q = self.find(u), self.find(v)
        # check if already in same group
        if p == q:
            return False
        # always merge smaller group in larger group
        if self.size[p] < self.size[q]:
            p, q = q, p
        self.parent[q] = p
        self.size[p] += self.size[q]
        return True
```

## Minimum Spanning Tree (MST)

- Tree extracted from a graph such that all nodes are included and the sum of
  weights of the edges included is minimum.

### Kruskal's Algorithm

```python
# consider the `DisjointSet` class from above

def kruskal_mst(n, edges):
    dsu = DisjointSet(n)
    edges.sort()
    mst_edges = list()

    for w, u, v in edges:
        if dsu.find(u) == dsu.find(v):
            continue
        dsu.union(u, v)
        mst_edges.append((u, v, w))

    return mst_edges
```

**Time Complexity**: `O(e*log(e))`  
**Space Complexity**: `O(n)`

## Topological Sort

- Applicable on a Directed Acyclic Graph (DAG).
- Consider that in a DAG, each directed edge represents a dependency relation
  between the nodes, i.e., if there is a directed edge from `u` to `v`, then
  `u` must be processed before `v`.
- In other words, all the nodes that produce an incoming edge on `v` must be
  processed before processing `v`.

### Kahn's algorithm

```python
def kahn_topological_sort(n, edges):
    graph = defaultdict(list)
    in_degree = [0] * n
    que = deque()
    ordering = []

    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1

    for v in range(n):
        if in_degree[v] == 0:
            que.append(v)

    while que:
        u = que.popleft()
        ordering.append(u)
        for v in graph[u]:
            in_degree[v] -= 1
            if in_degree[v] == 0:
                que.append(v)

    return ordering
```

- This can also be used to detect cycle in a directed graph.
- After the queue becomes empty, if we have covered all the nodes, then the
  graph is acyclic.

**Time Complexity**: `O(n)`  
**Space Complexity**: `O(n)`
