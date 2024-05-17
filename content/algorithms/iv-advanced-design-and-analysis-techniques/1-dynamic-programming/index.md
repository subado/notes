---
title: "1 Dynamic Programming(DP)"
date: 2023-12-23T17:51:57+04:00
draft: true
---

## Rod cutting

Steps to develop a dynamic-programming algorithm:

1. Characterize the structure of an optimal solution.
2. Recursively define the value of an optimal solution.
3. Compute the value of an optimal solution, typically in a bottom-up fashion.
4. Construct an optimal solution from computed information.

Steps 1–3 form the basis of a dynamic-programming solution to a problem.

**Optimal substructure**:
optimal solutions to a problem incorporate optimal solutions
to related subproblems, which we may solve independently.

The common solution of problems in which DP can be used
is the **recursive top-down implementation**, but in this case
our program likely will solve same subproblems multifoldly.

DP uses additional memory to save computation time(a **time-memory trade-off**).

In dynamic programming we saves solutions of solved subproblems
to prevent repeatedly solving of same subproblems.

2 ways:

1. **top-down with memoization**

   Results that computed previously are stored(cached) and
   will be returned next time instead of recompoting.

   The recursive procedure has been **memoized**.

2. **bottom-up method**

   Sort the subproblems by size and solve them in size order, smallest first.

### Subproblem graphs

(a “reduced” or “collapsed” version of the recursion tree for the top-down recursive method.)

The **sumproblem graph** has a directed edge from the vertex for subproblem `x` to
the vertex for subproblem `y` if determining an optimal solution for subproblem `x`
involve directly considering an optimal solution for subproblem `y`.

### Reconstructing a solution

To do this we need to record not only the optimal value
computed for each subproblem, but also a **choice**
that led to the optimal value.

### Exercises

- $$
  T(0) = 1 = 2^0\\\
  T(n) = 1 + (2^n - 1)\\\
  T(n) = 2^n
  $$

- example:

| length i | price p[i] | p[i]/i |
| -------- | ---------- | ------ |
| 1        | 1          | 1      |
| 2        | 5          | 2.5    |
| 3        | 8          | 2.66   |
| 4        | 9          | 2.25   |

The greedy will choose length 3 and length 1(p[3]/3 is the biggest value) that aren't the max sum.

- BOTTOM-UP-CUT-ROD + c

```
BOTTOM-UP-CUT-ROD(p,n,c)
  let r[0..n] be a new array
  r[0] = 0
  for j = 1 to n
    q = -∞
    for i = 1 to j
      sum = p[i] + r[j - i]
      if j - i != 0
        sum -= c
      q = max(q, sum)
  r[j] = q
return r[n]
```

- Reconstructed MEMOIZED-CUT-ROD

```
MEMOIZED-CUT-ROD(p, n)
  let r[0..n] and s[0..n] be a new arrays
  for i = 0 to n
    r[i] = -∞

  m = MEMOIZED-CUT-ROD-AUX(p, n, r, s)
  let v be a new vector
  while n > 0
    print v.add(s[n])
    n -= s[n]
  return m and v

MEMOIZED-CUT-ROD-AUX(p, n, r, s)
  if r[n] >= 0
    return r[n]
  if n == 0
    q = 0
  else q = -∞
    for i = 1 to n
      res = p[i] + MEMOIZED-CUT-ROD-AUX(p, n - i, r, s)
      if res > q
        q = res
        s[n] = i
  r[n] = q
  return q
```

- The Fibonacci

```
FIBONACCI(n)
    let fib[0..n] be a new array
    fib[0] = 1
    fib[1] = 1
    for i = 2 to n
        fib[i] = fib[i - 1] + fib[i - 2]
    return fib[n]
```

The subproblem graph has $n + 1$ vertices and $2n - 2$ edges.

$n = 0$ and $n = 1$ have $0$ edjes, but other vertices have $2n$ edjes.

## 2 Matrix-chain multiplication

$$
P(n) =
  \begin{cases}
  1 &\text{if }n = 1,\\\
  \displaystyle \sum_{k=1}^{n-1} P(k) P(n - k) &\text{if }n \ge 2.
  \end{cases}
$$

The sequence of **Catalan numbers**, which grows as $\Omega\left(\frac{4^n}{n^{3/2}}\right)$

### Exercises

1. $((A_{1}A_{2})((A_{3}A_{4})(A_{5}A_{6}))) = 2010$

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

#define INF  numeric_limits<ll>::max()
#define NINF numeric_limits<ll>::min()

pair<vector<vector<ll>>, vector<vector<ll>>> matrix_chain_order(const vector<ll> &dimensions)
{
  ll n = dimensions.size() - 1;
  vector<vector<ll>> costs(n, vector<ll>(n));
  vector<vector<ll>> splits(n, vector<ll>(n));

  for (ll i = 0; i < n; ++i)
  {
    costs[i][i] = 0;
  }

  for (ll len = 2; len <= n; ++len)
  {
    for (ll i = 0; i <= n - len; ++i)
    {
      ll j = i + len - 1;
      costs[i][j] = INF;
      for (ll split = i; split < j; ++split)
      {
        ll cost = costs[i][split] + costs[split + 1][j] +
                  dimensions[i] * dimensions[split + 1] * dimensions[j + 1];
        if (cost < costs[i][j])
        {
          costs[i][j] = cost;
          splits[i][j] = split;
        }
      }
    }
  }

  return {costs, splits};
}

void print_optimal_parens(const vector<vector<ll>> &splits, ll i, ll j)
{
  if (i == j)
  {
    cout << 'A' << "_{" << (i + 1) << '}';
  }
  else
  {
    cout << '(';
    print_optimal_parens(splits, i, splits[i][j]);
    print_optimal_parens(splits, splits[i][j] + 1, j);

    cout << ')';
  }
}

int main()
{
  vector<ll> dimension = {5, 10, 3, 12, 5, 50, 6};
  auto [costs, splits] = matrix_chain_order(dimension);
  cout << costs[0][5] << '\n';
  print_optimal_parens(splits, 1 - 1, 6 - 1);
  return 0;
}
```

2. MATRIX-CHAIN-MULTIPLY

```
MATRIX-MULTIPLY(A, B)
  if A.columns != B.rows
    error "incompatible dimensions"
  else
    let C be a new A.rows x B.columns matrix
    for i = 1 to A.rows
      for j = 1 to B.columns
        c[i, j] = 0
        for k = 1 to A.columns
          c[i, j] = c[i, j] + a[i, k] * b[k, j]
  return C

MATRIX-CHAIN-MULTIPLY(A, s, i, j)
  if i == j
    return A[i]
  if i + 1 == j
    return MATRIX-MULTIPLY(A[i], A[j])
  else
    MATRIX-MULTIPLY(MATRIX-CHAIN-MULTIPLY(A, s, i, s[i, j]), MATRIX-CHAIN-MULTIPLY(A, s, s[i, j], j))
```
