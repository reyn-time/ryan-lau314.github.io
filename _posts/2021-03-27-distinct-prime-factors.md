---
layout: post
title: "Problem 047: Distinct Prime Factors"
date: 2021-03-27 09:45:36 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The first two consecutive numbers to have two distinct prime factors are:

- $14 = 2 \times 7$
- $15 = 3 \times 5$

The first three consecutive numbers to have three distinct prime factors are:

- $644 = 2^2 \times 7 \times 23$
- $645 = 3 \times 5 \times 43$
- $646 = 2 \times 17 \times 19$

Find the first four consecutive integers to have four distinct prime factors each. What is the first of these numbers?

## Solution
To solve the problem, we simply need a function to count the number of distinct prime factors of any given number. 

I used the [`primefactors` function](https://docs.sympy.org/latest/modules/ntheory.html#sympy.ntheory.factor_.primefactors) from the very helpful library `sympy`. Thanks sympy!

There is no point in reinventing the wheels if the provided method is efficient enough. Over-optimisation takes too much time away from me... I have more important things to do.

{% highlight python %}
from sympy.ntheory import primefactors
from itertools import count

streak = 0

for n in count(647):
    prime_factor_count = len(primefactors(n))

    if prime_factor_count == 4:
        streak += 1
    else:
        streak = 0

    if streak == 4:
        print(f"Found: {n - 3} - {n}")
        break
{% endhighlight %}