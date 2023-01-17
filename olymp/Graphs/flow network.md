#### Flow network

A **network** is a directed graph $G = (V, E)$ with a non-negative capacity function $c$ for each edge. If an edge $(u, v) \in E$ we may assume that $(v, u)$ is also a member of $E$, otherwise we might add the edge and set $c(v, u) = 0$.

There are two distinguished nodes — source $s$ and sink $t$, $s \neq t$. Now $(G, c, s, t)$ is called a **flow network**.

#### Flow

Let’s consider the capacity type as some field $\mathbb{F}$ (useful cases are $\mathbb{F} = \mathbb{R}$ and $\mathbb{F} = \mathbb{Z}$) 

Flow function is a function $f: E \rightarrow \mathbb{R}$ with given properties
- Capacity constraints — $0 \leq f(e) \leq c(e)$
- Skew symmetry — $f(u, v) = -f(v, u)$
- Flow conservation

$$
    conservation(v) = \sum_{u \in V} f(v, u) - \sum{t \in V} f(t, v) = \begin{cases}
    |f|, v = s \\
    -|f|, v = t \\
    0, v \notin \{s, t\}
    \end{cases}
$$

Flow value is the outgoing flow of source or flow into the sink — $|f| = conservation(s) = -conservation(t)$

#### Maximum flow problem

Find a flow $f$, such that there is no flow $f^*$ such that $|f^*| > |f|$

#### $s-t$ cut

A cut is a pair of sets $(A, B)$ such that $V = A \cup B$, $s \in A$, $t \in B$

The capacity of an $s-t$ cut is the sum of capacities of all edges in the cut — $\sum c(u, v)$ over all $u \in A$, $v in B$.

The flow of an $s-t$ cut is equal to $\sum \left( f(u, v) - f(v, u) \right)$ over all $u \in A$, $v in B$. It’s not more than the capacity because of capacity constraints.

Minimal cut (min cut) — an $s-t$ cut with minimal capacity.