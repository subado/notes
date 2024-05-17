---
title: "4 Divide and Conquer"
date: 2023-12-29T17:48:06+04:00
draft: true
---

1. **Divide** the problem into a number of subproblems
   that are smaller instances of the same problem.

2. **Conquer** the subproblems by solving them recursively.
   If the subproblem sizes are small enough, however,
   just solve the subproblems in a straightforward manner.

3. **Combine** the solutions to the subproblems into the solution for the original problem.

When the subproblems are large enough to solve recursively, we call that the **recursive case**.

The subproblem that size is small enough is **the base case**.

### Recurrences

A **recurrence** is an equation or inequality that
describes a function in terms of its value on smaller inputs.

In the **substitution method**, we guess a bound and then
use mathematical induction to prove our guess correct.

The **recursion-tree** method converts the recurrence into a tree whose nodes
represent the costs incurred at various levels of the recursion.
We use techniques for bounding summations to solve the recurrence.

The **master method** provides bounds for recurrences of the form:

$$
T(n) = a \times T\left(\frac{n}{b}\right) + f(n)
$$

`a` number of subproblems, each of which is
`1/b` the size of the original problem.

### Technicalities in recurrences

We neglect certain technical details when we state and solve recurrences.

## 1 The maximum-subarray problem

### Exercises

1. It will return the greates element of the array.

2. The brute-force method.

```
FIND-MAXIMUM-SUBARRAY(A, low, high)
  max-sum = -∞
  for left = low to high
    sum = 0
    for right = left to high
    sum += A[right]
    if sum > max-sum
      max-sum = sum
      max-left = left
      max-right = right
  return (max-left, max-right, max-sum)
```

3. Code to compare brute-force and recursive methods.

```cpp
#include <bits/stdc++.h>
#include <vector>
using namespace std;

typedef long long ll;
struct Subarray
{
  ll low;
  ll high;
  ll sum;
};

#define INF  numeric_limits<ll>::max()
#define NINF numeric_limits<ll>::min()

template <typename Array>
Subarray find_max_crossing_subarray(Array &a, ll low, ll mid, ll high)
{
  ll left_sum = NINF, sum = 0;
  ll max_left;
  for (ll l = mid; l >= low; --l)
  {
    sum += a[l];
    if (sum > left_sum)
    {
      left_sum = sum;
      max_left = l;
    }
  }
  ll right_sum = NINF;
  sum = 0;
  ll max_right;
  for (ll r = mid + 1; r <= high; ++r)
  {
    sum += a[r];
    if (sum > right_sum)
    {
      right_sum = sum;
      max_right = r;
    }
  }
  return {max_left, max_right, left_sum + right_sum};
}

template <typename Array>
Subarray find_maximum_subarray(Array &a, ll low, ll high)
{
  if (low == high) // base case
  {
    return {low, high, a[low]};
  }

  ll mid_index = (low + high) / 2;

  auto left = find_maximum_subarray(a, low, mid_index);
  auto right = find_maximum_subarray(a, mid_index + 1, high);
  auto mid = find_max_crossing_subarray(a, low, mid_index, high);

  auto m = max({left.sum, right.sum, mid.sum});
  if (m == left.sum)
  {
    return left;
  }
  else if (m == right.sum)
  {
    return right;
  }
  return mid;
}

template <typename Array>
Subarray brute_force_find_maximum_subarray(Array &a, ll low, ll high)
{
  ll max_sum = NINF;
  ll max_low, max_high;
  max_low = max_high = low;

  for (ll l = low; l <= high; ++l)
  {
    ll sum = 0;
    for (ll h = l; h <= high; ++h)
    {
      sum += a[h];
      if (sum > max_sum)
      {
        max_sum = sum;
        max_low = l;
        max_high = h;
      }
    }
  }
  return {max_low, max_high, max_sum};
}

template <typename Array>
void randomize_array(Array &a, mt19937 &rng, uniform_int_distribution<mt19937::result_type> &dist)
{
  for (auto &i : a)
  {
    auto n = dist(rng);
    if (n <= 100)
    {
      n *= -1;
    }

    i = n;
  }
}

template <typename T>
chrono::microseconds measure_execution_time(T f)
{
  std::chrono::steady_clock::time_point begin = std::chrono::steady_clock::now();
  f();
  std::chrono::steady_clock::time_point end = std::chrono::steady_clock::now();
  return chrono::duration_cast<std::chrono::microseconds>(end - begin);
}

int main()
{
  random_device dev;
  mt19937 rng(dev());
  uniform_int_distribution<mt19937::result_type> dist(0, 200);
  for (ll n = 2; n < INF; ++n)
  {
    vector<ll> a(n);
    randomize_array(a, rng, dist);
    vector<ll> b = a;
    auto brute = measure_execution_time(
      bind(brute_force_find_maximum_subarray<vector<ll>>, a, 0, a.size() - 1));
    auto recursive_time =
      measure_execution_time(bind(find_maximum_subarray<vector<ll>>, b, 0, b.size() - 1));
    if (brute.count() > recursive_time.count())
    {
      cout << n << endl;
      break;
    }
  }
  return 0;
}
```

Code to compare time of 3 algorithms

```cpp
#include <bits/stdc++.h>
#include <vector>
using namespace std;

typedef long long ll;
struct Subarray
{
  ll low;
  ll high;
  ll sum;
};

#define INF  numeric_limits<ll>::max()
#define NINF numeric_limits<ll>::min()
#define N0   37

template <typename Array>
Subarray find_max_crossing_subarray(Array &a, ll low, ll mid, ll high)
{
  ll left_sum = NINF, sum = 0;
  ll max_left;
  for (ll l = mid; l >= low; --l)
  {
    sum += a[l];
    if (sum > left_sum)
    {
      left_sum = sum;
      max_left = l;
    }
  }
  ll right_sum = NINF;
  sum = 0;
  ll max_right;
  for (ll r = mid + 1; r <= high; ++r)
  {
    sum += a[r];
    if (sum > right_sum)
    {
      right_sum = sum;
      max_right = r;
    }
  }
  return {max_left, max_right, left_sum + right_sum};
}

template <typename Array>
Subarray find_maximum_subarray(Array &a, ll low, ll high)
{
  if (low == high) // base case
  {
    return {low, high, a[low]};
  }

  ll mid_index = (low + high) / 2;

  auto left = find_maximum_subarray(a, low, mid_index);
  auto right = find_maximum_subarray(a, mid_index + 1, high);
  auto mid = find_max_crossing_subarray(a, low, mid_index, high);

  auto m = max({left.sum, right.sum, mid.sum});
  if (m == left.sum)
  {
    return left;
  }
  else if (m == right.sum)
  {
    return right;
  }
  return mid;
}

template <typename Array>
Subarray brute_force_find_maximum_subarray(Array &a, ll low, ll high)
{
  ll max_sum = NINF;
  ll max_low, max_high;
  max_low = max_high = low;

  for (ll l = low; l <= high; ++l)
  {
    ll sum = 0;
    for (ll h = l; h <= high; ++h)
    {
      sum += a[h];
      if (sum > max_sum)
      {
        max_sum = sum;
        max_low = l;
        max_high = h;
      }
    }
  }
  return {max_low, max_high, max_sum};
}

template <typename Array>
Subarray modified_find_maximum_subarray(Array &a, ll low, ll high)
{
  if (low == high) // base case
  {
    return {low, high, a[low]};
  }
  else if (low - high + 1 <= N0)
  {
    return brute_force_find_maximum_subarray(a, low, high);
  }

  ll mid_index = (low + high) / 2;

  auto left = find_maximum_subarray(a, low, mid_index);
  auto right = find_maximum_subarray(a, mid_index + 1, high);
  auto mid = find_max_crossing_subarray(a, low, mid_index, high);

  auto m = max({left.sum, right.sum, mid.sum});
  if (m == left.sum)
  {
    return left;
  }
  else if (m == right.sum)
  {
    return right;
  }
  return mid;
}

template <typename Array>
void randomize_array(Array &a, mt19937 &rng, uniform_int_distribution<mt19937::result_type> &dist)
{
  for (auto &i : a)
  {
    auto n = dist(rng);
    if (n <= 100)
    {
      n *= -1;
    }

    i = n;
  }
}

template <typename T>
chrono::microseconds measure_execution_time(T f)
{
  std::chrono::steady_clock::time_point begin = std::chrono::steady_clock::now();
  f();
  std::chrono::steady_clock::time_point end = std::chrono::steady_clock::now();
  return chrono::duration_cast<std::chrono::microseconds>(end - begin);
}

int main()
{
  random_device dev;
  mt19937 rng(dev());
  uniform_int_distribution<mt19937::result_type> dist(0, 200);
  for (ll n = 2; n < N0 * 2; ++n)
  {
    vector<ll> a(n);
    randomize_array(a, rng, dist);
    vector<ll> b, c;
    b = c = a;
    auto brute = measure_execution_time(
      bind(brute_force_find_maximum_subarray<vector<ll>>, a, 0, a.size() - 1));
    auto recursive_time =
      measure_execution_time(bind(find_maximum_subarray<vector<ll>>, b, 0, b.size() - 1));
    auto modified_time =
      measure_execution_time(bind(modified_find_maximum_subarray<vector<ll>>, c, 0, c.size() - 1));

    cout << n << ":\n"
         << "\tbrute = " << brute.count() << "\n"
         << "\trecursive = " << recursive_time.count() << "\n"
         << "\tmodified = " << modified_time.count() << "\n";
    if (n == N0)
    {
      cout << "CROSSOVER POINT\n";
    }
  }
  return 0;
}
```

5. linear-time algorithm

```
FIND-MAXIMUM-SUBARRAY(A)
  max-sum = -∞
  sum = 0

  for j = 1 to A.length
    sum += A[j]
    if sum > max-sum
      max-sum = sum
      max-high += 1
    elseif A[j] > max-sum
      max-sum = A[j]
      max-low = max-high = j
  return (max-low, max-high, max-sum)
```

## 2 Strassen’s algorithm for matrix multiplication

$$
T =
  \begin{cases}
  \Theta(1) &\text{if } n = 1 \\\
  7 \times T\left(\frac{n}{2}\right) + \Theta(n^2) &\text{if } n > 1
  \end{cases}
$$

### Exercises

1. calculation

$$
C =
  \begin{pmatrix}
  18 & 14 \\\
  62 & 66
  \end{pmatrix}
$$

2. pseudocode

```
DIVIDE-INTO-SUBMATRICES(A)
  n = A.rows
  let submatrices[2, 2][2] be a new 2 x 2 matrix of array of points
  half = floor(n/2)

  submatrices[1, 1][1].row = 1
  submatrices[1, 1][1].column = 1
  submatrices[1, 1][2].row = half
  submatrices[1, 1][2].column = half

  submatrices[1, 2][1].row = 1
  submatrices[1, 2][1].column = half + 1
  submatrices[1, 2][2].row = half
  submatrices[1, 2][2].column = n

  submatrices[2, 1][1].row = half + 1
  submatrices[2, 1][1].column = 1
  submatrices[2, 1][2].row = n
  submatrices[2, 1][2].column = half

  submatrices[2, 2][1].row = half + 1
  submatrices[2, 2][1].column = half + 1
  submatrices[2, 2][2].row = n
  submatrices[2, 2][2].column = n

  return submatrices


STRASSEN(A, B)
  n = A.rows
  half = floor(n/2)
  let C be a new n x n matrix
  if n == 1
    c[1, 1] = a[1, 1] * b[1, 1]

  A = DIVIDE-INTO-SUBMATRICES(A)
  B = DIVIDE-INTO-SUBMATRICES(B)
  C = DIVIDE-INTO-SUBMATRICES(C)

  let S[10] be a new array of half x half matrices

  S[1] = B[1, 2] - B[2, 2]
  S[2] = A[1, 1] + A[1, 2]
  S[3] = A[2, 1] + A[2, 2]
  S[4] = B[2, 1] - B[1, 1]
  S[5] = A[1, 1] + A[2, 2]
  S[6] = B[1, 1] + B[2, 2]
  S[7] = A[1, 2] - A[2, 2]
  S[8] = B[2, 1] + B[2, 2]
  S[9] = A[1, 1] - A[2, 1]
  S[8] = B[1, 1] + B[1, 2]

  let P[7] be a new array of half x half matrices
  P[1] = STRASSEN(A[1, 1], S[1])
  P[2] = STRASSEN(S[2], B[2, 2])
  P[3] = STRASSEN(S[3], B[1, 1])
  P[4] = STRASSEN(A[2, 2], S[4])
  P[5] = STRASSEN(S[5], S[6])
  P[6] = STRASSEN(S[7], S[8])
  P[7] = STRASSEN(S[9], S[10])

  C[1, 1] = P[5] + P[4] - P[2] + P[6]
  C[1, 2] = P[1] + P[2]
  C[2, 1] = P[3] + P[4]
  C[2, 2] = P[5] + P[1] - P[3] - P[7]

  return C
```

## 3 The substitution method for solving recurrences

The **substitution method** for solving recurrences comprises two steps:

1. Guess the form of the solution.
2. Use mathematical induction to find the constants
   and show that the solution works.

### Making a good guess

You can use

- recursion trees,
- some heuristics.

You can rove loose upper and lower bounds on
the recurrence and then reduce the range of uncertainty.

### Subtleties

You should revise the guess by subtracting a lower-order term when you hit a math snag.

### Avoiding pitfalls

We need to prove the exact form of the inductive hypothesis.

### Changing variables

A changing variables can make an unknown recurrence similar to one you have seen before.
