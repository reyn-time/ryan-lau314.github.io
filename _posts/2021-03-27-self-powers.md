---
layout: post
title: "Problem 048: Self Powers"
date: 2021-03-27 10:23:49 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The series, $1^1 + 2^2 + 3^3 + ... + 10^{10} = 10405071317$.

Find the last ten digits of the series, $1^1 + 2^2 + 3^3 + \cdots + 1000^{1000}$.

## Solution
The goal is to find the answer to $\left(1^1 + ... + 1000^{1000}\right) \bmod N$, where $N$ = 10,000,000,000. 

This is equivalent to finding the solution to 
\\\[\left(\sum_{i=1}^{1000} \left(i^i \bmod N\right)\right) \bmod N\\\] 

The key here is to compute $i^i \bmod N$ efficiently for any given $i$ and $N$. This is known as *modular exponentiation*.

Python internally provides [an efficient way](https://docs.python.org/3.2/library/functions.html#pow) of computing such exponentiations. `pow(i, j, N)` gives the value of $i^j \bmod N$ in $\mathcal{O}(\log(j))$ time. For more information on how it is implemented, check [this Wikipedia article](https://en.wikipedia.org/wiki/Modular_exponentiation#Right-to-left_binary_method).

The solution becomes rather straightforward if you know how to use the function properly:

{% highlight python %}
MODULUS = 10_000_000_000

sum = 0
for i in range(1, 1001):
    sum = (sum + pow(i, i, MODULUS)) % MODULUS

print(sum)
{% endhighlight %}