# CS 330 Lecture 4: Dynamic Programming

## Fibonacci Numbers

> Apparently this is based on rabbits giving birth lol

`F(0) = 1, F(1) = 1, ∀n≥2 F(n) = F(n-1) + F(n-2)`

Q: How to compute F(n)? 

*Recursive solution*

```
Fib(n):
  if n≤1 then return 1
  return Fib(n-1) + Fib(n-2)
```