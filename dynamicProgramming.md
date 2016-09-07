# CS 330 Lecture 4: Dynamic Programming

## Fibonacci Numbers

> Apparently this is based on rabbits giving birth lol

`F(0) = 1, F(1) = 1, ∀n≥2 F(n) = F(n-1) + F(n-2)`

Q: How to compute F(n)? 

### Recursive solution

```
Fib(n):
  if n≤1 then return 1
  return Fib(n-1) + Fib(n-2)
```

#### Running time?

ex. Call Fib(7)

Row    | 1      | 2                  | 3    
---    | ---    | ---                | ---
Calls  | Fib(7) | Fib(6), **Fib(5**) | **Fib(5)**, Fib(4), Fib(4), Fib(3) 

(Wasting time calling for info you already have)

```
T(n) = T(n-1) + T(n-2) + O(1)
     = O( (root(5) + 1) / 2 )^n )
```

*Note: you do not have to know how to get this*

### Memorized Search

> Tries to avoid doing intermediate work by remembering intermediate results

```
Fib(n)
  if n≤1 return 1
  if n is "solved" return solution(n) 
  r = f(n-1) + f(n-2)
  mark n as solved, solution[n] = r
  return r 
```
#### Solution Table

0   | 1   | 2   | 3   | ...
--- | --- | --- | --- | --- 
1   | 1   | 2   | 3   | ...

*Note: can accomplish this with a hash table (see LeetCode's Two Sum one-pass lookup table solution)*

Table has n-1 elements in it. 

#### Running Time

```
T(n) = O(# of elements in table (n) * amount of time spent per entry)
     = O(n * 1) 
     = O(n)
```

### Iterative Solution

> All I have to do is fill in all the entries of the table. Even better, we can just go from left to right without having to do any recursive calculations! (Improvement on Memorized Search)

```
Fib(n)
  if n≤1 return 1
  F(0) = F(1) = 1 // where F is the lookup table
  for i = 2 to n
    F(i) = F(i-1) + F(i-2)
  return F(n)
```

### Dynamic Programming (DP)

#### General idea

> Save intermediate results to avoid repeated computation 

#### Design a DP algorithm

> 1. Identify important subproblems (ie make a table, list, matrix, etc.) to remember problems & solutions
> 2. Fill in table entries in a 'good' order 

## Shortest Path in Directed Acyclic Graphs

> Directed acyclic graph (DAG)

* **Directed** = arrows on edges (one-way)
* **Acyclic** = can never go in loops/revisit old nodes

![DAG](https://upload.wikimedia.org/wikipedia/commons/4/4b/Directed_acyclic_graph.svg)

* **Source** = vertex with no edges directed towards it (possible to have multiple sources)
* Edges generally will have weights 

### Sample DAG Problem

> Given a DAG, edge (i,j) with length W(i,j), want to find length of shortest path from s to t

### Recursive Solution

**Think: What is the last step of the solution?**

> Then, you can *undo* that step and give yourself a simpler subproblem!

ex. We know that the last step is going from **E** to **F**.

```
shortest(v) (length of shortest path from s to v)
  if v = s return 0
  return min(shortest(u) + W(u,v))
```

### Memorized Search

```
shortest(v)
  if v = s return 0
  if v is "solved" return distance[v]
  r = inf
  for u = 1 to n
    if (u,v) is an edge, shortest(u) + W(u,v) < r
      r = shortest(u) + W(u,v)
  mark v as solved, distance[v] = r
  return r
```

#### Solution Table (see class notes for graph)

vertex    |s   | a   | b   | c   | d   | t
---       |--- | --- | --- | --- | --- | ---
distance  | 0  | 4   | 5   | 7   | 6   |  14

Note: if graph is *not* acyclic, you'll have an inf loop!

## Longest Common Subsequence (LCS)

> Input: 2 sequencs a = 1,2,3,2,1 b = 2,3,1,4,1

**Subsequence:** subset of elements in the same order (not necessarily continuous)

```
ex. a = 1,2,3,2,1

  1,2,3, ok
  1,3,2 ok
  1,2,1 ok
  1,1,3 no
  
```

### Sample LCS Problem

> Find the length of the LCS of a, b (a = 1,2,3,2,1 b = 2,3,1,4,1)

> (in this case, the LCS is 2,3,1)

(recall: look at the **last step** of the solution!)

a = 1,2,3,2,**1**

b = 2,3,1,4,**1**




