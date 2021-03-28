---
layout: post
title: "Problem 050: Consecutive prime sum"
date: 2021-03-28 16:23:24 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The prime 41, can be written as the sum of six consecutive primes:

\\\[41 = 2 + 3 + 5 + 7 + 11 + 13\\\]

This is the longest sum of consecutive primes that adds to a prime below one-hundred.

The longest sum of consecutive primes below one-thousand that adds to a prime, contains 21 terms, and is equal to 953.

Which prime, below one-million, can be written as the sum of the most consecutive primes?

## Solution
For simplicity let $N$ = 1,000,000 (a million). The problem can be solved using the following psuedocode:

- Set $P$ to be the list of primes under $N$.
- For $l$ from $\left\vert P \right\vert$ to 1 inclusive:
  - For $i$ from 0 to $\left\vert P \right\vert - l$ inclusive:
    - Let $S = P[i] + P[i + 1] + \cdots + P[i + l - 1]$.
    - If $S \in P$:
        - Return $(S, i, l)$.

The $S$ returned is the prime which can be written as the sum of most consecutive primes: 
\\\[S = P[i] + P[i+1] + \cdots + P[i+l-1]\\\]

The list of primes can be easily found using the `primerange` function from `sympy`. 

The only difficult subproblem is to compute the subarray sum $P[i] + P[i+1] + \cdots + P[j]$ efficiently for all $i$, $j$.

Thankfully there is a constant-time solution for this. First we have to precompute a list $A$, in which:
\\\[
    A[i] = P[0] + P[1] + \cdots + P[i]  
\\\]

Such list can be precomputed in linear time:
{% highlight python %}
A = [0] * len(P)

prev = 0
for i in range(0, len(P)):
  A[i] = prev + P[i] 
  prev = A[i]
{% endhighlight %}

Python provides some built-in functions to make our lives even easier:

{% highlight python %}
from itertools import accumulate

A = list(accumulate(P))
{% endhighlight %}

We can the find the sum of any subarray sums with the following equation:
\\\[ 
    P[i] + P[i+1] + \cdots + P[j] = 
    \begin{cases} 
        A[j] &(\text{if $i=0$})\\\\\
        A[j] - A[i-1] &(\text{otherwise})
    \end{cases}
\\\]

Here is the full solution in Python. I made a few optimisations too.
{% highlight python %}
from sympy import sieve
from itertools import accumulate

MAX_VALUE = 1_000_000

prime_list = [i for i in sieve.primerange(1, MAX_VALUE)]
prime_acc = [0] + list(accumulate(prime_list))
prime_set = set(prime_list)


def p050():
    n = len(prime_list)
    largest_prime = prime_list[n - 1]

    for l in range(n, 0, -1):
        for i in range(0, n - l + 1):
            prime_sum = sublist_sum(i, i + l - 1)

            # Terminate early when the prime sum is larger than the largest prime.
            # Prime sums at later i's are larger, therefore not helpful.
            if prime_sum > largest_prime:
                break

            # Use set instead of list for faster searching
            if prime_sum in prime_set:
                return (prime_sum, i, l)


def sublist_sum(i, j):
    if i == 0:
        return prime_acc[j]
    return prime_acc[j] - prime_acc[i - 1]
{% endhighlight %}