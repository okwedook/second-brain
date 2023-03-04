#### Definition of an independent set

In a simple undirected [graph](graph) $G = (V, E)$ an **independent set** is a set of vertices $A$, such that there is no edge between any two vertices in $A$. More formally $A$ is an independent set iff $\forall_{v \in A} \forall_{u \in A}\ (v, u) \notin E$.

#### Maximum independent subset

Since all subsets of an independent set $A$ are independent sets the problem is usually formulated as a maximization problem. That is, find an independent set $A$ with the largest number of vertices in it.