---
title: "5 Number Theoretic Algorithms"
date: 2024-01-05T21:11:23+04:00
draft: true
---

## 1 Elementary number-theoretic notions

### Divisibility and divisors

$d | a$ ("$d$ **divides** $a$") where $a = kd$.

$a$ is a **multiple** of $d$.
$d$ is a **divisor** of $a$.

The **trivial divisors** are $1$ and $a$.
The **nontrivial divisors** of $a$ are the **factors** of $a$.

### Prime and composite numbers

A **prime number** has only **trivial divisors**.

A **composite number** is any integer that isn't a prime.

The integer $1$ is a **unit**.

### The division theorem, remainders, and modular equivalence

**Division theorem**

> For any integer $a$ and any positive integer $n$, there exist
> unique integers $q$ and $r$ such that $0 \le r < n$ and $a = qn + r$.

$q = \lfloor a/n \rfloor$ is **quotient**.
$r = a \mod n$ is **remainder(residue)**.

$n | a \text{ if } a \mod n = 0$.

The **equivalence class modulo n** containing an integer $a$ is

$$
[a]_{n} = { a + kn : k \in \mathbb{Z} }.
$$

$$
\mathbb{Z}_{n} = { [a]_{n} : 0 \le a \le n - 1 }.
$$

### Common divisors and greatest common divisors

If $d$ is a divisor of $a$ and $d$ is also a divisor of $b$, then $d$ is a **common divisor** of $a$ and $b$.

$$
d | a \text{ and } d | b \text{ implies } d | (ax + by)
$$

$$
a | b \text{ and } b | a \text{ implies } a = \pm b.
$$

The **greatest common divisor** of two integers $a$ and $b$,
not both zero, is the largest of the common divisors of $a$ and $b$.

$$
gcd(a, b) = gcd(b, a),\\\
gcd(a, b) = gcd(-a, b),\\\
gcd(a, b) = gcd(|a|, |b|),\\\
gcd(a, 0) = |a|,\\\
gcd(a, ka) = |a| \text{ for any } k \in \mathbb{z}.
$$

$$
\text{If }a\text{ and }b\text{ are any integers, not both zero, then }gcd(a, b)\text{ is the smallest positive}\\\
\text{element of the set }\\{ax + by : x, y \in \mathbb{Z}\\}\text{ of linear combinations of }a\text{ and }b.
$$

$$
\begin{cases}
  d | a \\\
  d | b \\\
\end{cases} \rArr d | gcd(a, b).
$$

$$
gcd(an, bn) = n \times gcd(a, b).
$$

$$
\begin{cases}
  n | ab \\\
  gcd(a, n) = 1 \\\
\end{cases} \rArr n | b.
$$

### Relatively prime integers

Two integers $a$ and $b$ are **relatively prime** if $gcd(a, b) = 1$.

$$
\begin{cases}
  gcd(a, p) = 1 \\\
  gcd(b, p) = 1 \\\
\end{cases} \rArr gcd(ab, p) = 1
$$

### Unique factorization

$$
\text{For all primes }p\text{ and all integers }a\text{ and }b
\text{, if } p | ab \text{, then } p | a \text{ or } p | b \text{ (or both)}.
$$

$$
\textbf{(Unique factorization)}\\\
\text{There is exactly one way to write any composite integer }a\text{ as a product of the form}\\\
a = p_{1}^{e_{1}}p_{2}^{e_{2}}...p_{r}^{e_{r}}\\\
\text{where the }p_{i}\text{ are prime, }p_{1} < p_{2} < ... < p_{r}\text{ , and the }e_{i}\text{ are positive integers.}
$$
