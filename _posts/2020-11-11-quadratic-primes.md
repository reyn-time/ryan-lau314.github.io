---
layout: post
title: "Problem 027: Quadratic Primes"
date: 2020-11-11 19:33:21 +0000
tags: euler
---

{% include mathjax.html %}

## Problem

Euler discovered the remarkable quadratic formula:
\\\[n^2 + n + 41\\\]

It turns out that the formula will produce 40 primes for the consecutive integer values $0 \leq n \leq 39$. However, when $n=40$, $40^2 + 40 + 41 = 40 (40 + 1) + 41$ is divisible by 41, and certainly when $n=41$, $41^2 + 41 + 41$ is clearly divisible by 41.

The incredible formula $n^2 - 79n + 1601$ was discovered, which produces 80 primes for the consecutive values $0 \leq n \leq 79$. The product of the coefficients, −79 and 1601, is −126479.

Considering quadratics of the form:
\\\[n^2 + an + b\\\]

, where $\mathopen|a\mathclose| < 1000$, $\mathopen|b\mathclose| \leq 1000$ and $\mathopen|n\mathclose|$ is the modulus/absolute value of $n$, e.g. $\mathopen|11\mathclose| = 11$ and $\mathopen|-4\mathclose| = 4$.
<p>
Find the product of the coefficients, $a$ and $b$, for the quadratic expression that produces the maximum number of primes for consecutive values of $n$, starting with $n=0$.
</p>

## Solution
Let $f_{(a,b)}$ be the function where $f_{(a,b)}(n) = n^2 + an + b$. 

Mathematically, we would like to find the largest natural number $N$ such that $f_{(a,b)}(n)$ is prime for all natural number $n < N$. Here's a possible way of finding this $N$:
{% highlight python %}
def consecutive_prime_count(a, b):
    """
    Consider the expression f(n) = n^2 + an + b.

    The function returns the largest N such that f(n) is prime for all n between 0 and N-1 inclusive.
    """
    n = 0
    while is_prime(n * n + a * n + b):
        n += 1
    return n
{% endhighlight %}

To find the optimal $f_{(a,b)}$ that generates the most number of primes, we can just go through all possible values of $(a, b)$. 

By noting that $f(0) = b$, we can just go through the cases where $b$ is a prime. (Why?)

The code I wrote looks something like this:
{% highlight python %}
possible_b = list(filter(lambda value: is_prime(value), range(1, 1000)))
max_consecutive_prime_count = 0
optimal_a = 0
optimal_b = 0

for a in range(-999, 1000):
    for b in possible_b:
        count = consecutive_prime_count(a, b)
        if count > max_consecutive_prime_count:
            max_consecutive_prime_count = count
            optimal_a, optimal_b = a, b

print(f"optimal_a * optimal_b = {optimal_a * optimal_b}")
{% endhighlight %}

A time-efficient `is_prime` function is crucial to the performance of the algorithm.

Since the function is frequently used here, I just pre-computed all primes below 2,001,000 and checked if the input number belongs to the set:

{% highlight python %}
all_primes = set()
for index, num_of_factor in enumerate(sieve_of_eratosthenes(2001000)):
    if num_of_factor == 0 and index >= 2:
        all_primes.add(index)

def is_prime(n):
    return n in all_primes
{% endhighlight %}

Why 2,001,000 you ask? That's because $a, b$ are not larger than 1000. The largest prime that we will be testing, therefore, is bounded above by $f_{(1000, 1000)}(1000) = 2001000$. There are certainly better upper bounds, but I am too lazy to think of one right now.