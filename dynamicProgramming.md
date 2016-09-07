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
Solution Table

0   | 1   | 2   | 3   | ...
--- | --- | --- | --- | --- 
1   | 1   | 2   | 3   | ...

*Note: can accomplish this with a hash table (see LeetCode's Two Sum one-pass lookup table solution)*

#### Running Time

```
T(n) = O(# of elements in table (n) * amount of time spent per entry)
     = O(n * 1) 
     = O(n)
```
