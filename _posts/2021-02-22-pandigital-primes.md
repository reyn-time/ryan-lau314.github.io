---
layout: post
title: "Problem 041: Pandigital Prime"
date: 2021-02-22 20:00:38 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
We shall say that an $n$-digit number is pandigital if it makes use of all the digits 1 to $n$ exactly once. For example, 2143 is a 4-digit pandigital and is also prime.

What is the largest $n$-digit pandigital prime that exists?

## Solution
The following [divisibility rule](https://en.wikipedia.org/wiki/Divisibility_rule#Divisibility_by_3_or_9) may be useful:
> A number is divisible by 3 if and only if the sum of its digits is divisible by 3.

The proof is relatively simple. Consider arbitrary number $N = d_1 \mathbin\Vert d_2 \mathbin\Vert\cdots\mathbin\Vert d_n$. 

\\\[ N = 10^{n-1}d_1 + 10^{n-2}d_2 + \cdots + d_n \equiv 1^{n-1}d_1 + 1^{n-2}d_2 + \cdots + d_n \equiv \sum_{i=1}^n d_i \pmod{3} \\\]

A 9-digit pandigital number must have a digit sum of $1 + 2 + \cdots + 9 = 45$, which is a multiple of 3. Hence there are no 9-digit pandigital primes. With similar reasonings, all pandigital primes can only have a length of 1, 4 or 7. It is probably safe to assume that the largest pandigital prime has 7 digits.

Here's my psuedocode:
* Set $s$ to `7654321`
* While not $prime(s)$:
  * Set $s$ to $prev\\_permutation(s)$
* Print $s$

I wrote posts about [checking primes]({% post_url 2020-11-11-quadratic-primes %}) and [finding the next permutation]({% post_url 2020-10-20-lexicographic-permutations %}) a few months ago. Though I can implement the functions again for this problem, I decided to use libraries like `itertools` and `sympy` in the end. I think I am starting to get lazier.

{% highlight python %}
from itertools import permutations
from functools import reduce
from sympy.ntheory import isprime

# Get the largest pandigital prime with n digits.
# Returns None if such prime does not exist.
def largest_pandigital_prime(n):
    # Exhausts all permutations of [1, 2, ..., n] in descending order.
    for tuple in permutations(reversed(range(1, n + 1))):
        number = tuple_to_number(tuple)
        if isprime(number):
            return number
    return None


# Concatenates digits [d_1, d_2, ..., d_n] into a number
def tuple_to_number(tuple):
    return reduce(lambda acc, digit: acc * 10 + digit, tuple)

print(largest_pandigital_prime(7))
{% endhighlight %}
