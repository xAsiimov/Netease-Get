# Graphs

## Basic Definitions and Applications

A graph consists of a collection V of nodes (or vertex) and a collection E of edges, each of which joins two of the nodes. Edges indicate a symmetric relationship between their ends in an undirected graph , and indicate an asymmetric relationships in a directed graph.

### Paths and Connectivity

Path in an undirected graph G = (V, E) is a sequence P of nodes $$v_{1}, v_{2}, ..., v_{k - 1}, v_{k}$$ with the property that each consecutive pair $$v_{i}, v_{i + 1}$$ is joined by an edge in G.

An undirected graph is connected if, for every pair of nodes u and v, there's a path from u to v. The distance between u and v is the minimum number of edges in a u-v path.

### Trees

An undirected graph is a tree if it's connected and doesn't contain a cycle. Every n-node tree has exactly $$n - 1$$ edges.

## Graph Connectivity and Graph Traversal

**s-t connectivity**: Find a path from s to t in the graph.

### Connected Component

The connected component of G containing s is the set of nodes that are reachable from the starting node s. BFS is one possible way to produce this component.

To find this connected component, we define $$R = {s}$$. While we find an edge `(u, v)` where $$u \in R$$ and $$v \notin R$$, add v to R.

The general algorithm to grow R is underspecified. Breadth-First Search and Depth-First Search are two natural ways of ordering the nodes we visit.

### Breadth-First Search

For each $$j \geq 1$$, layer $$L_{j}$$ produced by breadth-first search consists of all nodes at distance exactly j from s. There's a path from s to t if and only if t appears in some layer.

Let T be a breadth-first search tree. let x and y be nodes in T belonging to layers $$L_{i}$$ and $$L_{j}$$ respectively, and let (x, y) be an edge of G. Then i and j differ by at most 1.

### Depth-First Search

```py
visited = set()

def dfs(u):
  visited.add(u)
  for v in u.neighbors:
    if v not in visited:
      dfs(v)
```

For a given recursive call `dfs(u)`, all nodes that are marked "Explored" between the invocation and the end of this recursive call are descendants of u in T.

Let T be a depth-first search tree, where x and y be nodes in T. Let (x, y) be an edge of G that is not an edge of T. Then one of x or y is an ancestor of the other.

### The Set of All Connected Components

For any two nodes s and t in a graph, their connected components are either identical or disjoint.

## Testing Bipartiteness

The bipartite graph is where the node set V can be partitioned into sets X and Y in such a way that every edge has one end in X and the other end in Y. If a graph is bipartite, then it can't contain an odd cycle.

### Designing the Algorithm

Perform breadth-first search in the graph starting at node s, coloring s red, all of odd-numbered layer blue, all of even-numbered layer red. The total running time for the algorithm is $$O(V + E)$$.

### Analyzing the Algorithm

Let G be a coonnected graph, and let $$L_{1}$$, $$L_{2}$$, ... be the layers produced by breadth-first search starting at node s. Either one of the following two claims must hold.

- There's no edge of G joining two nodes of the same layer. In this case, G is a bipartite graph in which the node ein even-numbered layers can be colored red, and the nodes in odd-numbered layers can be colored blue.

- There's an edge of G joining two nodes of the same layer. In this case, G contains an odd-length cycle, so it can't be bipartite.

## Connectivity in Directed Graphs

In a directed graph, the edge `(u, v)` has a direction: it goes from `u` to `v`.

### Representation

To represent a directed graph for designing algorithms, we use a version of the adjancency list representation. Each node has two lists associated with it: one list for nodes to which it has edges, and another list for nodes from which it has edges.

### The Graph Search Algorithms

Breadth-first search and depth-first search are almost the same in directed graphs as they are in undirected graphs.

For breadth-first search, we start at a node `s`, define a first layer of nodes that all those to which `s` has an edge, and define a second layer that all those nodes to which the nodes in the first layer have an edge. The time complexity is $O(m + n)$.

In directed graphs, both breadth-first search and depth-first search compute a set of all nodes `t` that `s` has a path to `t`. Such nodes may or may not have paths back to `s`.

### Strong Connectivity

For two nodes `u` and `v`, if there's a path from `u` to `v` and a path from `v` to `u`, `u` and `v` are mutually reachable. The directed graph is strongly connected if every pair of nodes is mutually reachable.

If `u` and `v` are mutually reachable, `v` and `w` are mutually reachable, then `u` and `w` are mutually reachable.

To determine if a graph is strongly connected, we could run BFS in $$G$$ starting from `s`, and run BFS in $G^{rev}$ starting from `s`. If one of these two searches fails to reach every node, then $$G$$ is not strongly connected.

The algorithm above computes the set of all `v` such that `s` and `v` are mutually reachable from `s`, which is defined as the strong component containing the node `s`.

For any two nodes `s` and `t` in a directed graph, their strong components are either identical or disjoint.

## Directed Acyclic Graphs and Topological Ordering

If a directed graph has no cycles, it is defined as a directed acyclic graph, or a DAG. The DAGs can be used to encode precedence relations or dependencies in a natural way.

For each task `i` and `j`, a directed edge `(i, j)` is added whenever `i` must be done before `j`. If the resulting graph contains a cycle, there would be no way to do any of the tasks in the cycle.

The topological ordering of a graph is an ordering of its nodes $$v_1, v_2, \dots, v_n$$ so that every edge $$(v_i, v_j)$$, we have $$i < j$$. If the graph has a topological ordering, then the graph is a DAG.

### Compute the Topological Ordering

If G is a DAG, then G has a topological ordering. In every DAG, there is a node with no incoming edges.

To produce an efficient algorithm, we maintain two things:

- For each node `w`, the number of incoming edges that `w` has from active nodes
- The set `S` of all active nodes in `G` that have no incoming edges from other active nodes

At the start, all nodes are active, so we initialize the set `S` with a single pass through the nodes and edges. Then, each iteration consists of selecting a node `v` from the set `S`, deleting it, and decrement the number of active incoming edges for all neighbors of `v`. If the number of active incoming edges to `w` drops to 0, then we add `w` to the set `S`.
