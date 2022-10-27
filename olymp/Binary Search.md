### Task definition

Let's define $f(x) \rightarrow \{0, 1\}$ as a monotonic non-strictly increasing function on half interval $[l, r)$ and $f(l) = 0$ and $f(r) = 1$.  The task is to find the smallest $x$, such that $f(x) = 1$.

### Algorithm

The algorithm is as follows
1. Setup the range with $[l, r)$
1. Calculate delimiter $mid = \frac{(l + r)}{2}$
1. Look at the value of $f(mid)$
    - If $f(mid) = 0$, then all the values in range $[l, mid]$ are $0$ and the problem in range $[mid, r)$ is equivalent to the starting problem, but the range is two times shorter, so let us replace $l$ with value of $mid$
    - If $f(mid) = 1$, then all the values in range $[mid, r]$ are 1 and analogically range $[l, mid)$ is equivalent to the starting problem and is two times shorter, so let us replace $r$ with $mid$
1. If the current range $[l, r)$ is small enough to answer the problem precise enough, then stop the algorithm and return $r$ as the result. Otherwise repeat step 2.

### Stopping conditions (discrete case)

Range being small enough might differ from the nature of function $f$. If the function is discrete (values are only assigned at integer coordinates), then the current range is small enough in case there is exactly one element in $[l, r)$ which is the same as $r - l = 1$

Python code for discrete function $f$
```python
def binary_search(l, r):
    while r - l > 1:
        mid = (l + r) // 2
        if f(mid) == 0:
            l = mid
        else:
            r = mid
    return r
```
The time complexity of this algorithm is $\theta(\lceil log_2(r - l)\rceil)$ calls of $f$

### Stopping conditions (continuous case)

But sometimes we need to calculate the value for continuous functions. In this case we can only estimate the answer with given precision error $\varepsilon$ (is read as _[epsilon](https://en.wikipedia.org/wiki/Epsilon)_). We can do the same process until $r - l > eps$. Since the real answer is in range $[l, r)$ then returning answer $r$ will have an error at most $r - realans \leq r - l \leq \varepsilon$

Python code for continuous function $f$
```python
def binary_search(l, r, eps=1e-10):
    while r - l > eps:
        mid = (l + r) / 2
        if f(mid) == 0:
            l = mid
        else:
            r = mid
    return r
```