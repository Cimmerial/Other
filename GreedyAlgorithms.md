# Graph Algorithms Reference Guide

## Table of Contents
1. [Common Graph Representations](#representations)
2. [Breadth-First Search (BFS)](#bfs)
3. [Depth-First Search (DFS)](#dfs)
4. [Dijkstra's Algorithm](#dijkstra)
5. [Minimum Spanning Trees (MST)](#mst)
6. [Prim's Algorithm](#prims)
7. [Kruskal's Algorithm](#kruskals)
8. [Edge Cases & Pitfalls](#edge-cases)
9. [Performance Comparison](#performance)
10. [Examples](#examples)

## Graph Representations <a name="representations"></a>

### Adjacency Matrix
```python
# n x n matrix where matrix[i][j] represents edge from i to j
graph = [
    [0, 1, 0, 1],
    [1, 0, 1, 0],
    [0, 1, 0, 1],
    [1, 0, 1, 0]
]
```

### Adjacency List
```python
# Dictionary where keys are vertices and values are lists of neighbors
graph = {
    0: [1, 3],
    1: [0, 2],
    2: [1, 3],
    3: [0, 2]
}
```

## Breadth-First Search (BFS) <a name="bfs"></a>

### Key Characteristics
- Explores all vertices at current depth before moving to next level
- Uses queue data structure
- Finds shortest path in unweighted graphs
- Time Complexity: O(V + E)
- Space Complexity: O(V)

### Implementation
```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        vertex = queue.popleft()
        print(vertex, end=' ')  # Process vertex
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

### Finding Shortest Path
```python
def bfs_shortest_path(graph, start, end):
    visited = set()
    queue = deque([(start, [start])])
    
    while queue:
        vertex, path = queue.popleft()
        
        if vertex == end:
            return path
            
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    
    return None  # No path exists
```

### BFS Applications
1. Shortest path in unweighted graphs
2. Level-order tree traversal
3. Finding connected components
4. Web crawling
5. Social network connections (friends at distance k)

## Depth-First Search (DFS) <a name="dfs"></a>

### Key Characteristics
- Explores as far as possible along each branch before backtracking
- Uses stack (or recursion)
- Time Complexity: O(V + E)
- Space Complexity: O(V) for call stack

### Recursive Implementation
```python
def dfs_recursive(graph, vertex, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(vertex)
    print(vertex, end=' ')  # Process vertex
    
    for neighbor in graph[vertex]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
```

### Iterative Implementation
```python
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    
    while stack:
        vertex = stack.pop()
        if vertex not in visited:
            visited.add(vertex)
            print(vertex, end=' ')  # Process vertex
            
            # Add neighbors in reverse order for same traversal as recursive
            for neighbor in reversed(graph[vertex]):
                if neighbor not in visited:
                    stack.append(neighbor)
```

### Cycle Detection
```python
def has_cycle(graph):
    visited = set()
    rec_stack = set()
    
    def dfs_cycle(vertex):
        visited.add(vertex)
        rec_stack.add(vertex)
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                if dfs_cycle(neighbor):
                    return True
            elif neighbor in rec_stack:
                return True
                
        rec_stack.remove(vertex)
        return False
    
    for vertex in graph:
        if vertex not in visited:
            if dfs_cycle(vertex):
                return True
    return False
```

### DFS Applications
1. Topological sorting
2. Finding strongly connected components
3. Maze solving
4. Cycle detection
5. Path finding in game trees

## Dijkstra's Algorithm <a name="dijkstra"></a>

### Key Characteristics
- Finds shortest path in weighted graphs
- Works with non-negative weights only
- Time Complexity: O((V + E) log V) with binary heap
- Space Complexity: O(V)

### Implementation with Priority Queue
```python
import heapq

def dijkstra(graph, start):
    distances = {vertex: float('infinity') for vertex in graph}
    distances[start] = 0
    pq = [(0, start)]
    previous = {vertex: None for vertex in graph}
    
    while pq:
        current_distance, current_vertex = heapq.heappop(pq)
        
        # Skip if we've found a better path
        if current_distance > distances[current_vertex]:
            continue
            
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                previous[neighbor] = current_vertex
                heapq.heappush(pq, (distance, neighbor))
    
    return distances, previous

def get_path(previous, start, end):
    path = []
    current = end
    
    while current is not None:
        path.append(current)
        current = previous[current]
    
    return path[::-1] if path[0] == start else None
```

### Example Usage
```python
# Graph represented as adjacency dict with weights
graph = {
    'A': {'B': 4, 'C': 2},
    'B': {'A': 4, 'C': 1, 'D': 5},
    'C': {'A': 2, 'B': 1, 'D': 8, 'E': 10},
    'D': {'B': 5, 'C': 8, 'E': 2},
    'E': {'C': 10, 'D': 2}
}

distances, previous = dijkstra(graph, 'A')
path = get_path(previous, 'A', 'E')
print(f"Shortest path: {path}")
print(f"Distance: {distances['E']}")
```

## Minimum Spanning Trees (MST) <a name="mst"></a>

### Key Characteristics
- Connects all vertices with minimum total edge weight
- Contains no cycles
- Has exactly V-1 edges for V vertices
- May not be unique if multiple edges have same weight

## Prim's Algorithm <a name="prims"></a>

### Key Characteristics
- Grows MST one vertex at a time
- Better for dense graphs
- Time Complexity: O((V + E) log V) with binary heap
- Space Complexity: O(V)

### Implementation
```python
def prims(graph, start):
    mst = []
    visited = set([start])
    edges = [
        (weight, start, to)
        for to, weight in graph[start].items()
    ]
    heapq.heapify(edges)
    
    while edges:
        weight, frm, to = heapq.heappop(edges)
        
        if to not in visited:
            visited.add(to)
            mst.append((frm, to, weight))
            
            for next_vertex, next_weight in graph[to].items():
                if next_vertex not in visited:
                    heapq.heappush(edges, (next_weight, to, next_vertex))
    
    return mst
```

## Kruskal's Algorithm <a name="kruskals"></a>

### Key Characteristics
- Processes edges in order of increasing weight
- Better for sparse graphs
- Time Complexity: O(E log E)
- Space Complexity: O(V)

### Implementation with Union-Find
```python
class UnionFind:
    def __init__(self, vertices):
        self.parent = {v: v for v in vertices}
        self.rank = {v: 0 for v in vertices}
    
    def find(self, item):
        if self.parent[item] != item:
            self.parent[item] = self.find(self.parent[item])
        return self.parent[item]
    
    def union(self, x, y):
        xroot, yroot = self.find(x), self.find(y)
        if xroot == yroot:
            return
        if self.rank[xroot] < self.rank[yroot]:
            self.parent[xroot] = yroot
        elif self.rank[xroot] > self.rank[yroot]:
            self.parent[yroot] = xroot
        else:
            self.parent[yroot] = xroot
            self.rank[xroot] += 1

def kruskals(graph):
    edges = []
    for u in graph:
        for v, weight in graph[u].items():
            edges.append((weight, u, v))
    
    edges.sort()  # Sort edges by weight
    uf = UnionFind(graph.keys())
    mst = []
    
    for weight, u, v in edges:
        if uf.find(u) != uf.find(v):
            uf.union(u, v)
            mst.append((u, v, weight))
    
    return mst
```

## Edge Cases & Pitfalls <a name="edge-cases"></a>

### Common Edge Cases
1. **Empty Graph**
   ```python
   if not graph:
       return []  # or appropriate empty result
   ```

2. **Disconnected Components**
   ```python
   def find_components(graph):
       components = []
       visited = set()
       
       for vertex in graph:
           if vertex not in visited:
               component = set()
               dfs_component(graph, vertex, visited, component)
               components.append(component)
       
       return components
   ```

3. **Negative Cycles (Dijkstra)**
   ```python
   # Check for negative edges
   if any(weight < 0 for edges in graph.values() for weight in edges.values()):
       raise ValueError("Negative edges not allowed in Dijkstra's algorithm")
   ```

4. **Self-Loops**
   ```python
   def remove_self_loops(graph):
       return {v: {u: w for u, w in edges.items() if u != v}
               for v, edges in graph.items()}
   ```

5. **Parallel Edges (MST)**
   ```python
   def get_min_edges(graph):
       min_edges = {}
       for u in graph:
           for v, weight in graph[u].items():
               key = tuple(sorted([u, v]))
               if key not in min_edges or weight < min_edges[key]:
                   min_edges[key] = weight
       return min_edges
   ```

### Special Cases
1. **Complete Graphs**
   - All vertices connected to all others
   - MST algorithms can be optimized
   - BFS/DFS reach all vertices from any start

2. **Bipartite Graphs**
   ```python
   def is_bipartite(graph):
       colors = {}
       for vertex in graph:
           if vertex not in colors:
               colors[vertex] = 0
               if not dfs_bipartite(graph, vertex, colors):
                   return False
       return True
   ```

3. **Trees**
   - No cycles
   - Exactly V-1 edges
   - Any edge removal disconnects graph

## Performance Comparison <a name="performance"></a>

| Algorithm | Time Complexity | Space Complexity | Best For |
|-----------|----------------|------------------|----------|
| BFS | O(V + E) | O(V) | Shortest path (unweighted) |
| DFS | O(V + E) | O(V) | Path finding, cycles |
| Dijkstra | O((V + E) log V) | O(V) | Shortest path (weighted) |
| Prim's | O((V + E) log V) | O(V) | Dense graphs MST |
| Kruskal's | O(E log E) | O(V) | Sparse graphs MST |

## Examples <a name="examples"></a>

### Social Network Analysis
```python
def find_friends_at_distance(graph, start, distance):
    """Find all friends exactly 'distance' connections away."""
    if distance < 1:
        return set()
        
    visited = set([start])
    current_level = {start}
    
    for _ in range(distance):
        next_level = set()
        for person in current_level:
            for friend in graph[person]:
                if friend not in visited:
                    next_level.add(friend)
                    visited.add(friend)
        current_level = next_level
        
    return current_level
```

### Maze Solving
```python
def solve_maze(maze, start, end):
    """
    Solve maze using DFS.
    maze: 2D array where 0 is path and 1 is wall
    """
    def get_neighbors(pos):
        row, col = pos
        for dr, dc in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
            r, c = row + dr, col + dc
            if (0 <= r < len(maze) and 
                0 <= c < len(maze[0]) and 
                maze[r][c] == 0):
                yield (r, c)
    
    def dfs(pos, path, visited):
        if pos == end:
            return path
        
        for next_pos in get_neighbors(pos):
            if next_pos not in visited:
                visited.add(next_pos)
                result = dfs(next_pos, path + [next_pos], visited)
                if result:
                    return result
                visited.remove(next_pos)
        
        return None
    
    return dfs(start, [start], {start})
```

### Network Routing
```python
def find_redundant_paths(graph, source, dest, k=2):
    """Find k different paths between source and destination."""
    paths = []
    visited = set()
    
    def dfs_paths(vertex, path):
        if len(paths) >= k:
            return
            
        if vertex == dest:
            paths.append(path[:])
            return
            
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                dfs_paths(neighbor, path + [neighbor])
                visited.remove(neighbor)
    
    visited.add(source)
    dfs_paths(source, [source])
    return paths
```
