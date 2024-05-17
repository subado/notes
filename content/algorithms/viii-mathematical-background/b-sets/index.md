---
title: "Sets"
date: 2023-12-29T18:44:35+04:00
draft: true
---

## 1 Sets

A **set** is a collection of distinguishable objects, called its **members** or **elements**

Two sets are **equal** if they contain the same elements.\

If all the elements of a set `A` are contained in a set `B`,
then we say that `A` is a **subset** of `B`.
A set `A` is a **proper subset** of `B`.

**Set operations**:

- **intersection**

  `A ∩ B = {x : x ∈ A and x ∈ B}`

- **union**

  `A ∪ B = {x : x ∈ A or x ∈ B}`

- **difference**

  `A - B = {x : x ∈ A and x ∉ B`

**Laws**:

- **empty**

  `A ∩ ∅ = ∅`
  `A ∪ ∅ = A`

- **idempotency**

  `A ∩ A = A`
  `A ∪ A = A`

- **commutative**

  `A ∩ B = B ∩ A`
  `A ∪ B = B ∪ A`

- **associative**

  `A ∩ (B ∩ C) = (A ∩ B) ∩ C`
  `A ∪ (B ∪ C) = (A ∪ B) ∪ C`

- **distributive**

  `A ∩ (B ∪ C) = (A ∩ B) ∪ (A ∩ C)`
  `A ∪ (B ∩ C) = (A ∪ B) ∩ (A ∪ C)`

- **absorption**

  `A ∩ (A ∪ B) = A`
  `A ∪ (A ∩ B) = A`

- **DeMorgan's**

  `A - (B ∩ C) = (A - B) ∪ (A - C)`
  `A - (B ∪ C) = (A - B) ∩ (A - C)`

Often, all the sets under consideration are subsets
of some larger set `U` called the **universe**.

**Complement** of a set `A` as `A'` is `U`-`A`=`{x : x ∈ U and x ∉ A}`

`A'` = `A`

`A ∩ A' = ∅`

`A ∪ A' = U`

`(B ∩ C)' = B' ∪ C'`
`(B ∪ C)' = B' ∩ C'`

Two sets `A` and `B` are **disjoint** if they
have no elements in common, that is, if `A ∩ B = ∅`.

A collection `C = {S[i]}` g of nonempty sets forms a **partition** of a set `S` if

- the sets are **pairwise disjoint**, that is, `S[i], S[j] ∈ C` and `i != j` imply `S[i] ∩ S[j] = ∅`,

- their union is `S`, that is
  ```
  S = union(S[i] ∈ C, S[i])
  ```

The number of elements in a set is the **cardinality** (or **size**) of the set, denoted `|S|`.

If the cardinality of a set is a natural number,
we say the set is **finite**; otherwise, it is **infinite**.
An infinite set that can be put into a one-to-one correspondence with the natural
numbers `N` is **countably infinite**; otherwise, it is **uncountable**.

For any two finite sets `A` and `B`:

`|A ∪ B| = |A| + |B| - |A ∩ B|`
`|A ∪ B| <= |A| + |B|`

A subset of `k` elements of a set is sometimes called a **k-subset**.

The **power set** of `S` as `2**S` is the set of all subsets of a set `S`,
including the empty set and `S` itself.

The power set of a finite set `S` has cardinality `2**S`.

An ordered pair of two elements `a` and `b` is `(a, b) = {a, {a, b}}`.
`(a, b) != (b, a)`

The **Cartesian product** of two sets `A` and `B` is `A * B = {(a, b) : a ∈ A and b ∈ B`.
