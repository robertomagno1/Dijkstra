# Dijkstra’s Algorithm - A Shortest Path Approach

This repository provides an implementation of [Dijkstra’s Algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) along with a detailed mathematical explanation. Dijkstra’s algorithm is a classic graph search method that solves the shortest path problem for weighted graphs with nonnegative edge weights.

## Mathematical Foundations

**Given:**
- A weighted graph \( G = (V, E) \) where each edge \((u,v) \in E\) has a nonnegative weight \( w(u,v) \geq 0 \).
- A starting (source) node \( s \in V \).

**Goal:**  
Find the shortest path from \( s \) to every other vertex \( v \in V \)
.

### Initialization

We define a distance function \( d(v) \) that holds the shortest known distance from the start node \( s \) to the node \( v \). Initialize:


$$ d(v) = \begin{cases}
0 & \text{if } v = s \\[6pt]
\infty & \text{otherwise}
\end{cases} $$

This means initially we know the shortest path to the start node itself is 0, and we have no knowledge of paths to other nodes, hence they are set to infinity.

### Relaxation Step

Dijkstra’s algorithm proceeds by iteratively selecting the node with the smallest temporary distance, say \( u \), that hasn’t been "finalized" yet. Then, for each neighbor \( v \) of \( u \):

1. Compute a new tentative distance if we go via \( u \):
   
   $\text{alt} = d(u) + w(u,v)$
   

2. If \(\text{alt} < d(v)\), we update:
   
   $d(v) := \text{alt}.$

This process is called **relaxation**, as it may reduce the current known shortest distance to \( v \).

### Algorithm Outline

1. Initialize distances as above.
2. Set all nodes as unvisited.  
3. Choose the unvisited node with the smallest known distance. Let this node be \( u \).
4. Update (relax) the distances to all neighbors of \( u \).
5. Mark \( u \) as visited (the shortest path to \( u \) is now finalized).
6. Repeat steps 3–5 until all nodes are visited or no smaller tentative distance can be found.

When the algorithm finishes, \( d(v) \) will hold the shortest distance from \( s \) to \( v \).

### Complexity

If we use a priority queue (min-heap) to efficiently select the unvisited node with the smallest distance at each step, the complexity is:

$O(|E| \log |V|)$

where \(|V|\) is the number of vertices and \(|E|\) is the number of edges in the graph.

## Code Example

```python
import heapq

def dijkstra(graph, start):
    """
    Run Dijkstra's Algorithm on a given graph from a start node.

    Parameters
    ----------
    graph : dict
        Dictionary where keys are nodes and values are lists of (neighbor, weight) tuples.
        Example:
        graph = {
            'A': [('B', 2), ('C', 5)],
            'B': [('A', 2), ('C', 1), ('D', 4)],
            'C': [('A', 5), ('B', 1), ('D', 2)],
            'D': [('B', 4), ('C', 2)]
        }

    start : hashable
        The starting node for the shortest path calculations.

    Returns
    -------
    dist : dict
        Shortest distance from 'start' to each node.
    prev : dict
        Predecessor map to reconstruct the shortest paths.
    """
    # Initialize distances to infinity except the start node
    dist = {node: float('inf') for node in graph}
    dist[start] = 0
    prev = {node: None for node in graph}

    # Priority queue for selecting the node with smallest distance
    pq = [(0, start)]

    while pq:
        current_dist, u = heapq.heappop(pq)

        # If this is not the most current distance, skip it
        if current_dist > dist[u]:
            continue

        # Relax distances to neighbors
        for v, w in graph[u]:
            alt = dist[u] + w
            if alt < dist[v]:
                dist[v] = alt
                prev[v] = u
                heapq.heappush(pq, (alt, v))

    return dist, prev

# Example usage:
if __name__ == "__main__":
    graph_example = {
        'A': [('B', 2), ('C', 5)],
        'B': [('A', 2), ('C', 1), ('D', 4)],
        'C': [('A', 5), ('B', 1), ('D', 2)],
        'D': [('B', 4), ('C', 2)]
    }

    distances, predecessors = dijkstra(graph_example, 'A')
    print("Shortest Distances:", distances)
    print("Predecessors:", predecessors)

**Pseudocode:**
