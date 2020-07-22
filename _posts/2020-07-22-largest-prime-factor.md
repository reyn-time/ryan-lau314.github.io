---
layout: post
title: "Problem 003: Largest Prime Factor"
date: 2020-07-22 21:21:44 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The prime factors of 13195 are 5, 7, 13 and 29.

What is the largest prime factor of the number 600851475143?

## Solution
A prime factorization algorithm is required to solve this problem.

Below is the algorithm that I have written. The algorithm iterates through all possible factors. The input number is continuously divided by the divisible factors, until it becomes 1. 

It can be proven that the factors retrieved by the algorithm must be primes. (Why?)

{% highlight python %}
def prime_factorisation(n):
    """
    Returns the prime factor form of a number.
    For example, input number 100 = 2^2 * 5^2 will be 
    transformed to [(2, 2), (5, 2)].
    """
    factors = []
    curr_factor = 2
    exponent = 0
    while n > 1:
        while n % curr_factor == 0:
            n /= curr_factor
            exponent += 1

        if exponent > 0:
            factors.append((curr_factor, exponent))

        curr_factor += 1
        exponent = 0
    return factors
{% endhighlight %}

If we run `prime_factorization(600851475143)`, it will return `[(71, 1), (839, 1), (1471, 1), (6857, 1)]`. This implies that:

$$ 600851475143 = 71^1 \cdot 839^1 \cdot 1471^1 \cdot 6857^1 $$

Therefore the answer is 6857. 

There are two ways to improve on my algorithm. 
* By noting that all primes apart from 2 is odd, we can set `curr_factor` to 3 and increment `curr_factor` by 2 in the main iteration (Take special care to the case when the number is divisible by 2).
* If we have iterated to the point where `curr_factor * curr_factor > n`, we can be certain `n` itself is a prime (Why?). We can terminate the iteration early and put `(n, 1)` in the `factors` array.