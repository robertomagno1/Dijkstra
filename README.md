# Dijkstra's Algorithm

Dijkstra's algorithm is a graph search algorithm that solves the shortest path problem for weighted graphs with nonnegative edge weights.  
It computes the shortest path from a source node \( s \) to all other nodes in a weighted graph \( G = (V, E) \), where:
- \( V \) is the set of vertices (or nodes).
- \( E \) is the set of edges, each with a nonnegative weight \( w(u,v) \geq 0 \).

## Mathematical Explanation

1. **Initialization:**
   We start by assigning an initial distance value to every node in the graph. Let \( d(v) \) be the shortest known distance from \( s \) to \( v \). Initially, set:
   $$
   d(v) = \begin{cases} 
   0 & \text{if } v = s \\[6pt]
   \infty & \text{if } v \neq s
   \end{cases}
   $$

   This means the distance to the source node $ s $is 0, and for all others, it's set to infinity (or some very large number).

2. **Set of Visited Nodes:**
   Maintain a set $S$ of nodes whose shortest path from $ s $ has been found. Initially, $ S = \emptyset$

3. **Relaxation Step:**
   At each iteration, we select a node \( u \notin S \) with the smallest tentative distance \( d(u) \). We then add \( u \) to \( S \).

   For each neighbor \( v \) of \( u \), we check if the current known distance \( d(v) \) is greater than \( d(u) + w(u,v) \). If yes, we update:
   \[
   d(v) := d(u) + w(u,v).
   \]

   This step is known as **relaxation**, as it potentially lowers the estimates of the shortest paths.

4. **Termination:**
   Repeat the relaxation process until all nodes are in \( S \) or no smaller tentative distance can be found. At the end, \( d(v) \) holds the shortest distance from \( s \) to \( v \).

## Algorithm Outline

- **Input:** A weighted graph \( G \) with nonnegative weights, a start node \( s \).
- **Output:** The shortest distances \( d(v) \) from \( s \) to every vertex \( v \).

**Pseudocode:**
