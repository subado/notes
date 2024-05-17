---
title: "C Counting and Probability"
date: 2023-12-29T22:26:24+04:00
draft: true
---

## 1 Counting

### Rules of sum and product

The **rule of sum**:

> The number of ways to choose one element from one
> of two disjoint sets is the sum of the cardinalities of the sets.

The **rule of product**:

> The number of ways to choose an ordered pair is the number of ways to
> choose the first element times the number of ways to choose the second element.

### Strings

A **string** over a finite set `S` is a sequence of elements of `S`.

A **substring** `s'` of a string `s` is an ordered sequence of consecutive elements of `s`.

### Permutations

A **permutation** of a finite set `S` is an ordered sequence of all
the elements of `S`, with each element appearing exactly once.

There are `n!` permutations of a set of `n` elements.

A `k-permutation` of `S` is an ordered sequence of `k` elements of `S`,
with no element appearing more than once in the sequence.

$$
n \times (n - 1) \times (n - 2)...(n - k + 1) = \frac{n!}{(n - k)!}
$$

### Combinations

A **k-combination** of an `n`-set `S` is simply a `k`-subset of `S`.

$$
\frac{n!}{k! \times (n - k)!}
$$

### Binomial coefficients

"n choose k"

$$
{n \choose k} = \frac{n!}{ k! \times (n - k)! }
$$

$$
{n \choose k} =  {n \choose n - k}
$$

The **binomial expansion**:

$$
\displaystyle \sum_{k=0}^{n}{n \choose k} x^k y^{n-k}
$$

$$
2^n = \displaystyle \sum_{k=0}^{n}{n \choose k}
$$
