---
title: "2 Getting Started"
date: 2023-12-22T17:58:09+04:00
draft: true
---

## 1 Insertion sort

```
INSERTION-SORT(A)
  for j = 2 to A.length
    key = A[j]
    // Insert A[j] into the sorted sequence A[1..j - 1].
    i = j - 1
    while i > 0 and A[i] > key
      A[i + 1] = A[i]
      i = i - 1
    A[i + 1] = key
```

**Insertion sort** is an efficient algorithm for sorting a small number of elements.

**Loop invariant**:
At the start of each iteration of the for loop of lines 1–8, the subarray
A[1..j-1] consists of the elements originally in A[1..j-1], but in sorted order.

3 things about a loop invariant:

- **Initialization**: It is true prior to the first iteration of the loop.

- **Maintenance**: If it is true before an iteration of the loop, it remains true before the
  next iteration.

- **Termination**: When the loop terminates, the invariant gives us a useful property
  that helps show that the algorithm is correct.

  (this property help to conclude that the algorithm is correct.)

Using loop invariants is like mathematical induction.

### Exercise

- A = {31, 41, 59, 26, 41, 58}

  iterations:

  1. 31 > 41? no -> {31, 41}
  2. 41 > 59? no -> {31, 41, 59}
  3. 59 > 26? yes -> {31, 41, 59, 59}
     41 > 26? yes -> {31, 41, 41, 59}
     31 > 26? yes -> {31, 31, 41, 59}
     -> {26, 31, 41, 59}
  4. 59 > 58? yes -> {26, 31, 41, 59, 59}
     41 > 58? no -> {26, 31, 41, 58, 59}

- nonincreasing

  ```
  INSERTION-SORT(A)
    for j = 2 to A.length
      key = A[j]
      // Insert A[j] into the sorted sequence A[1..j - 1].
      i = j - 1
      while i > 0 and A[i] > key
        A[i + 1] = A[i]
        i = i - 1
      A[i + 1] = key
  ```

- **linear search**

  ```
  LINEAR-SEARCH(A, v)
    for i = 1 to A.length
      if A[i] == v
        return i
    return NIL
  ```

  1. Initialization is true because if a[1] == v, then return i happens
  2. Maintenance is true because `for` loop works until we don't find a proper value and
     we check next element each iteration only if we didn't find it.
  3. Termination is happened when proper value is found or not found in the whole array and then NIL is returned.

- adding two n-bit binary integers

```
SUM(A, B)
  let C[1..n + 1] be a new array
  for i = 1 to n + 1
    C[i] = 0

  for i = 1 to A.length
    C[i] += A[i] + B[i]

    if C[i] > 1:
      C[i + 1] += 1
      C[i] -= 2
  return C
```

## 2 Analyzing algorithms

**Analyzing** an algorithm has come to mean predicting
the resources that the algorithm requires.

Before we can analyze an algorithm, we must have a
model of the implementation technology that we will use.

**Random-access machine (RAM) model** is a generic one-processor model of computation.

- instructions are executed one after another,
  with no concurrent operations

- contains instructions commonly found in real computers:
  arithmetic (such as add, subtract, multiply, divide, remainder, floor, ceiling), data
  movement (load, store, copy), and control (conditional and unconditional branch,
  subroutine call and return)

- integers are represented by c \* lg(n) bits for some constant c >= 1

### Analysis of insertion sort

A **input size** is _the number of items_ in the input or
the total _number of bits_ needed to represent the input in ordinary binary notation.

The **running time** of an algorithm on a particular input is the number of primitive
operations or “steps” executed

A running time as `a*n + b` for constants a and b that depend on
the statement costs `C[i]`; it is thus a **linear function** of n.

A running time as `a*(n**2) + b*n + c` for constants a, b, and c
that again depend on the statement costs `C[i]`;
it is thus a **quadratic function** of n.

### Worst-case and average-case analysis

The **worst-case running time** is the longest running time for any input of size n.

- For some algorithms, the worst case occurs fairly often.

- The algorithm will never take any longer that the worst-case running time(an upper bound).

- The "average case" is often roughly as bad as the worst case.

### Order of growth

The **rate of growth**, or **order of growth**, of the running time
is the leading term without constants of a running-time formula(`n`, `n**2`).

One algorithm to be more efficient than another if its
worst-case running time has a lower order of growth.

### Exercises

- `O(n**3)`

- selection sort

  ```
  SELECTION-SORT(A)
  for i = 1 to A.length - 1   // c1*n
    min = i                   // c2*(n - 1)
    for j = i + 1 to A.length // c3*(n - 1)*(n - i - 1)
      if A[j] < A[min]        // c4*(n - 1)*(n - i - 2)
        min = j               // c5*(n - 1)*(n - i - 2) = the worst-case; 0 = the best-case
    tmp = A[i]                // c6*(n - 1)
    A[i] = A[min]             // c7*(n - 1)
    A[min] = tmp              // c8*(n - 1)
  ```

  This algorithm maintain the loop invarian in the which A[1..i] is always sorted.

  It needs only n - 1 due to the fact, that in the n - 1 iteration A[n] and A[n - 1]
  will be compared and swapped if it's necessary.

  The best-case will be `O(n**2)` when the array is already sorted.
  `T(n) = c1*n + c2*(n - 1) + c3*(n - 1)*(n - i - 1) + c4*(n - 1)*(n - i - 2) + 0 + c6*(n - 1) + c7*(n - 1) + c8*(n - 1)`

  The worst-case will be `O(n**2)` when the array is in reverse sorted order.
  `T(n) = c1*n + c2*(n - 1) + c3*(n - 1)*(n - i - 1) + c4*(n - 1)*(n - i - 2) + c5*(n - 1)*(n - i - 2) + c6*(n - 1) + c7*(n - 1) + c8*(n - 1)`

- the average-case and worst-case running times of linear search.

  In the average-case element will be found in the middle of the array and running time will be `O(n/2)` or simple `O(n)`.

  In the worst-case element will be found in the end of the array and running time will be `O(n)`.

## 3 Designing algorithms

**incremental** approach.

### The divide-and-conquer approach

1. **Divide** the problem into a number of subproblems that
   are smaller instances of the same problem

2. **Conquer** the subproblems by solving them recursively.
   If the subproblem sizes are small enough, however, just solve
   the subproblems in a straightforward manner

3. **Combine** the solutions to the subproblems into
   the solution for the original problem.

```
MERGE(A, p, q, r)
  n1 = q - p + 1
  n2 = r - q
  let L[1..n1 + 1] and R[1..n2 + 1] be new arrays
  for i = 1 to n1
    L[i] = A[p + i - 1]
  for j = 1 to n2
    R[j] = A[q + j]

  L[n1 + 1] = +∞
  R[n2 + 1] = +∞
  i = 1
  j = 1
  for k = p to r
    if L[i] <= R[j]
      A[k] = L[i]
      i = i + 1
    else A[k] = R[j]
      j = j + 1

MERGE-SORT(A, p, r) // O(n * lg(n))
  if p < r
    q = floor(p + r/2)
    MERGE-SORT(A, p, q)
    MERGE-SORT(A, q + 1, r)
    MERGE(A, p, q, r)
```

### Analyzing divide-and-conquer algorithms

When an algorithm contains a recursive call to itself,
we can often describe its running time
by a **recurrence equation** or **recurrence**.

```
       { O(1)                       if n <= c,
T(n) = {
       { a*T(n/b) + D(n) + C(n)     otherwise.

a = number of subproblems.
b = each subroblem is 1/b the size of the original.
D(n) = time to divide the problem into subproblems.
C(n) = time to combine the solutions to the subproblems into
       the solution to the original problem.
```

### Exercises

---

A = {3, 41, 52, 26, 38, 57, 9, 49}

```
    [3, 9, 26, 38, 41, 49, 57]
        /                  \
 [3, 26, 41, 52]    [9, 38, 49, 57]
    /       \          /       \
[3, 41]  [26, 52]  [38, 57]  [9, 49]
 /   \     /   \     /   \    /   \
[3] [41] [52] [26] [38] [57] [9] [49]
```

---

MERGE without sentinels

```
MERGE(A, p, q, r)
  n1 = q - p + 1
  n2 = r - q
  index1 = p - 1
  index2 = q
  if n1 > n2
    swap(n1, n2)
    swap(index1, index2)

  let L[1..n1] and R[1..n2] be new arrays
  for i = 1 to n1
    L[i] = A[index1 + i]
  for j = 1 to n2
    R[j] = A[index2 + j]

  i = 1
  j = 1
  for k = p to r
    if k > q or R[j] < L[i]
      A[k] = R[j]
      j = j + 1
    elseif L[i] <= R[j]
      A[k] = L[i]
      i = i + 1
```

---

`T(n) = n * lg(n)` when n is an exact power of 2

```
T(2) = 2
T(n) = 2*T(n/2) + n

T(4) = 2 * T(2) + 4 = 8 = 4 * log(4, 2)

T(2**i) = 2 * T(2**(i-1)) + 2**i = 2**i * log(2**i, 2) = 2**i * i
T(2**(i + 1)) = 2 * 2**i * i + 2**(i + 1) = 2**(i + 1) * i + 2**(i + 1) =
= 2**(i + 1) * (i + 1) = 2**(i + 1) * log(2**(i + 1), 2)
```

---

```
RECURSIVE-INSERTION-SORT(A)
  INSERTION-SORT-AUX(A, A.length)

INSERTION-SORT-AUX(A, n)
  if n > 1
    INSERTION-SORT-AUX(A, n - 1)
    key = A[n]
    i = n - 1
    while i > 0 and A[i] > key
      A[i + 1] = A[i]
      i = i - 1
    A[i + 1] = key
```

```
       { O(1)             if n == 1
T(n) = {
       { T(n - 1) + O(n)  if n > 1
```

---

```
BINARY-SEARCH(A, v, l, r)
  if l > r
    return NIL

  mid = floor((l + r)/2)
  if A[mid] == v
    return mid
  elseif v < A[mid]
    return BINARY-SEARCH(A, v, l, mid - 1)
  else
    return BINARY-SEARCH(A, v, mid + 1, r)
```

```
       { O(1)       if n == 1
T(n) = {
       { T(n/2) + O(1)     if n > 1

The best case running time is O(1).


The worst case running time is O(lg(n)).
Prove by induction.


T(2) = 1 = log(2, 2)

T(2**i) = T(2**(i - 1)) + O(1) = log(2**i, 2) = i

T(2**(i + 1)) = T(2**i) + O(1) = i + O(1) = log(2**(i + 1), 2)
```

---

INSERTION-SORT with using BINARY-SEARCH won't improve worst-case running time due to the fact,
that the BINARY-SEARCH will help us to find the index where we should place a new element, but
we also need to move all elements higher than a new element.

```
j = index of a new element

Binary search takes O(lg(j - 1)) in the worst case.
Moving elements takes O(j - 1) in the worst case.

T(n) = n * ( O(lg(n - 1) + (O(n - 1) )
The worst time become even more worse.
```

---

```
FIND-SUM(S, x)
  MERGE-SORT(S)         // O(n*lg(n))
  for i = 1 to S.length
    j = BINARY-SEARCH(S, x - S[i], 0, S.length) // O(lg(n))
    if j != NIL
      return i and j
  return NIL
```

```
T(n) = O(n * lg(n)) + n * O(lg(n)) = O(n * lg(n))
```

## Problems

### 1 Insertion sort on small arrays in merge sort

1. `O(k**2)` is the worst case time to sort sublist of length `k`, but we have `n/k` sublists ->
   the worst case time is `O(k**2 * n/k)` = `O(nk)`.

2. Worst-case time of merging the sublists is `O(n * lg(n/k))` because
   we have `lg(n/k)` levels in the recursion tree and on each level
   we merge arrays that sum of lengths is `n`.

3. The max value of k is `lg(n)`.

4. Choose `k` be the largest length of sublist on which insertion sort is faster than merge sort.

### 2 Correctness of bubblesort

1. A' = sorted A.

2. inner loop

   **Initialization**. After first iteration of loop `A[j - 1]` is the smallest element of array `A[j - 1..j]`.

   **Maintenance**. On each iteration we compare `A[j - 1]` with `A[j]`
   and make `A[j - 1]` to be the smallest element of this pair.

   **Termination**. At the end of last iteration(`j = i + 1`)
   the smallest element of array `A[i + 1..A.length]` is `A[i]`.

3. outer loop

   **Initialization**. At the start of first iteration of loop we have `A[1..1]` sorted array.

   **Maintenance**. At the start of each iteretion we have `A[1..(i - 1)]` sorted array and
   at the end of each iteration we have `A[1..i]` sorted array.

   **Termination**. At the end of last iteration(`i = A.length - 1`) we have `A[1..(A.length - 1)]` but
   array `A[A.length..A.length]` has length `1` and because of this it is sorted. We have sorted array `A[1..A.length]`.

4. worst-case running time

```
T(n) = O((n - 1) * (n - i + 1)) = O(n**2) = worst-case running time of INSERTION-SORT.
```

### 3 Correctness of Horner’s rule

1. `T(n) = O(n)` because we have loop with `n` iterations.

2. naive polynomial-evaluation

```
NAIVE-HORNER()
  y = 0
  for k = 0 to n
    temp = 1
    for i = 1 to k
      temp = temp * x
      y = y + a[k] * temp
```

`T(n) = O(n**2)`

3. loop invariant

   **Initialization**. `i = n` -> y = 0

   **Maintenance**.

   **Termination**. At termination `i = -1` -> `y = sum(k=0, n, a[k]*x**k)`

4. The invariant of the loop is a sum that equals a polynomial with the given coefficients.

### 4 Inversions

1. {2, 3, 8, 6, 1}
   (2, 1)
   (3, 1)
   (8, 1)
   (6, 1)
   (8, 6)

2. deacrising sorted array and `((n - 1)*n)/2`

3. inner `while` loop of INSERTION-SORT will do `j - 1` operation to move all elements of `A[1..j]` array.
   And it is worst-case running time = `T(n) = O(n * (n - 1))` = `O(n**2)`

4. COUNT-INVERSIONS

```
MERGE-INVERSIONS(A, p, q, r)
    n1 = q - p + 1
    n2 = r - q
    let L[1..n1 + 1] and R[1..n2 + 1] be new arrays
    for i = 1 to n1
        L[i] = A[p + i - 1]
    for j = 1 to n2
        R[j] = A[q + j]
    L[n1 + 1] = +∞
    R[n2 + 1] = +∞
    i = 1
    j = 1
    inversions = 0
    for k = p to r
        if L[i] <= R[j]
            A[k] = L[i]
            i = i + 1
        else
            inversions = inversions + n1 - i + 1
            A[k] = R[j]
            j = j + 1
    return inversions


COUNT-INVERSIONS(A, p, r)
    if p < r
        q = floor((p + r) / 2)
        left = COUNT-INVERSIONS(A, p, q)
        right = COUNT-INVERSIONS(A, q + 1, r)
        inversions = MERGE-INVERSIONS(A, p, q, r) + left + right
        return inversions
```
