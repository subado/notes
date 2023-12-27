---
title: "1 The Role of Algorithms in Computing"
date: 2023-12-22T15:33:25+04:00
draft: true
---

## 1 Algorithms

(informally)
An **algorithm** is any well-defined computational procedure that
takes set of values, as input and produces set of values, as output.

An **algorithm** as a tool for solving a well-specified **computational problem**.

An **instance of a problem** consists of the input needed to compute a solution to the problem.

If, for every input instance, an algorithm halts with the
correct output then an algorithm is **correct**.

### What kinds of problems are solved by algorithms?

Characteristics that are common to
many interesting algorithmic problems:

1. They have many candidate solutions, the overwhelming majority
   of which do not solve the problem at hand.

2. They have practical applications
   (finding the shortest path).

### Data structures

A **data structure** is a way to store and organize
data in order to facilitate access and modifications.

It is important to know the strengths and limitations of several of data structures.

### Technique

Techniques of algorithm design and analysis make you capable to develop algorithms on your own.

### Hard problems

Although no efficient algorithm for an NP-complete problem has ever
been found, nobody has ever proven that an efficient algorithm for one cannot exist.

### Parallelism

We can liken these multicore computers to several sequential
computers on a single chip.

## 2 Algorithms as a technology

Algorithms that are efficient in terms of time or space
will help you to use computer resources wisely.

### Efficiency

By using an algorithm whose running time grows more slowly,
even with a poor hardware or software, this algorithm will be many times more efficient,
especially with the big set of data, than alogorithm whose running time grows more faster.

### Algorithms and other technologies

A **technology** is a summation of algorithms and hardware.

Algorithms are at the core of most technologies used in contemporary computers.

## Problems

For each function `f(n)` and time `t` in the following table, determine the largest
size n of a problem that can be solved in time t, assuming that the algorithm to
solve the problem takes f(n) _microseconds_.

|            | 1 second           | 1 minute           | 1 hour             | 1 day              | 1 month             | 1 year               | 1 century              |
| ---------- | ------------------ | ------------------ | ------------------ | ------------------ | ------------------- | -------------------- | ---------------------- |
| lg(n)      | 10\*\*1000000      | 10\*\*60000000     | 10\*\*3600000000   | 10\*\*86400000000  | 10\*\*2592000000000 | 10\*\*31104000000000 | 10\*\*3110400000000000 |
| sqrt(n)    | 10\*\*12           | 36\*(10\*\*14)     | 1296\*(10\*\*16)   | 746496\*(10\*\*16) | 6718464\*10\*\*18   | 967458816\*10\*\*18  | 967458816\*10\*\*22    |
| n          | 10\*\*6            | 6\*(10\*\*7)       | 36\*(10\*\*8)      | 864\*(10\*\*8)     | 2592\*(10\*\*9)     | 31104\*(10\*\*9)     | 31104\*(10\*\*11)      |
| n \* lg(n) | 189481             | 8.6493×10^6        | 4.17597×10^8       | 8.69288×10^9       | 2.28203×10^11       | 2.5 × 10^12          | 2.1608 × 10^14         |
| n\*\*2     | 1\* 10\*\*03       | 7.745967\*10\*\*03 | 6\*10\*\*04        | 2.939388\*10\*\*05 | 1.609969\*10\*\*06  | 5.577096\*10\*\*06   | 5.577096\*10\*\*07     |
| n\*\*3     | 1\*10\*\*02        | 3.914868\*10\*\*02 | 1.532619\*10\*\*03 | 4.420838\*10\*\*03 | 1.373657\*10\*\*04  | 3.14489\*10\*\*04    | 1.459728\*10\*\*05     |
| 2\*\*n     | 1.993157\*10\*\*01 | 2.583846\*10\*\*01 | 3.174535\*10\*\*01 | 3.633031\*10\*\*01 | 4.12372\*10\*\*01   | 4.482217\*10\*\*01   | 5.146602\*10\*\*01     |
| n!         | 9.44560891441633   | 11.1663556427872   | 12.7888433523223   | 13.9966466360747   |                     |                      |                        |
