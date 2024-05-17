---
title: "1 Elementary Graph Algorithms"
date: 2024-01-16T18:38:54+04:00
draft: true
---

### 1 Representations of graphs

$$
G = (V, E)
$$

**Sparse** graphs $|E| << |V|^2$
**Dense** grapth $|E| \approx |V|^2$.

The **adjacency-list** and the **adjacency matrix**.

The adjacency-list representation has the amount of memory $\Theta(V + E)$.

The adjacency matrix of a graph requires $\Theta(V^2)$ memory

**Weighted graphs**, that is, graphs for which each edge has an associated _weight_.

### 2 Breadth-first search

**Breadth-first** search is one of the simplest algorithms for searching a graph
and the archetype for many important graph algorithms.

```
BFS(G, s)
  for each vertex ∈ 2 G.V - {s}
    u.color = WHITE
    u.d = ∞
    u.p = NIL
  s.color = GRAY
  s.d = 0
  s.p = NIL
  Q = ∅
  ENQUEUE(Q, s)
  while Q !=  ∅
    u = DEQUEUE(Q)
    for each v ∈ G.Adj[u]
      if v.color == WHITE
        v.color = GRAY
        v.d = u.d + 1
        v.p = u
        ENQUEUE(Q, v)
    u.color = BLACK
```

Colors meanings:

- `WHITE`
  A vertice isn't discovered yet.
- `GRAY`
  A vertice is discovered but vertices connected with it aren't.
- `BLACK`
  A vertice and vertices connected with it are discovered.
