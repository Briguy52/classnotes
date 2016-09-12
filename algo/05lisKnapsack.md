# Algo Lecture 05: LIS and Knapsack

## Longest Increasing Subsequence

> input a = 1, 3, 2, 5, 6, 4

> output: longest increasing subsequence 

#### try: find subproblems

* look at a(n), is a(n) in the LIS?

* no ⇒ LIS = LIS(a[1, ... n-1]) 
* yes ⇒ find an increasing subsequence of a[1, ... n-1] whose last elem < 4 a(n)

`f[i] = LIS of a[1...i]` **NO** not working b/c do not know last elem

#### alternative subproblem:

`f[i] = length LIS for a[1...i] that stops at a[i]` 

> Cannot be arbitrary IS of a, its last elem must be 4! (a[i])

#### recursion

`f[i] = max f[j] + 1 where j < 1, a[j] < a[i]`

#### boundary conditions

`if for any j < i, a[j] ≥ a[i], then f[i] = 1`

```
for i = 1 to n
  f[i] = 1
  for j = 1 to i - 1
    if a[j] < a[i] and f[j] + 1 > f[i] 
      then f[i] = f[j] + 1

```

#### f[i] = LIS that ends at a[i]

```
a = 1 3 2 5 6 4
f = 1 2 2 3 4 3
```

## Knapsack Problem

> given n objects with int weights w(1) .. w(n), knapsack that can hold at most weight M

**goal:** find a subset of objects so that the total weight W ≤ M, W is also as close to M as possible (ie mac capacity)

### subproblem 

* case 1: the nth object is not in knapsack, remaining problem: capacity M, n-1 (same capacity, one less item)
* case 2: the nth object is in the knapsack, remaining problem: capacity M - w(n), n-1 objects 

`f[i,j] = max possible weight if capacity == i, first j objects`

`f[i,j] = max( f[i, j-1], f[i-w(j), j-1] + w(j) ) `

boundary f[i, 0] = 0 (no objects)
boundary f[0, j] = 0 (no capacity)

#### a variation

> object i has value v(i), want to find a subset of objects with total weight ≤ m, total value maximized

```
m = 10 
w(1) = 6, v(1) = 8 // has more value with smaller weight
w(2) = 7, v(2) = 6 // has more weight with smaller value

f[i,j] = max( f[i, j - 1], f[i - w(j), j - 1] + v(j) ) // can use w(j) instead of v(j) to opt. for weight!
```
