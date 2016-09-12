# Algo Lecture 05: 

## Longest Increasing Subsequence

> input a = 1, 3, 2, 5, 6, 4

> output: longest increasing subsequence 

try: find subproblems

* look at a(n), is a(n) in the LIS?

* no ⇒ LIS = LIS(a[1, ... n-1]) 
* yes ⇒ find an increasing subsequence of a[1, ... n-1] whose last elem < 4 a(n)

f[i] = LIS of a[1...i] **NO** not working b/c do not know last elem

alternative subproblem:

f[i] = length LIS for a[1...i] that stops at a[i] 

> Cannot be arbitrary IS of a, its last elem must be 4! (a[i])
