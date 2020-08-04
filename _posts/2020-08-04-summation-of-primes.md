---
layout: post
title: "Problem 010: Summation of Primes"
date: 2020-08-04 22:19:10 +0100
tags: euler
---
## Problem
{% include mathjax.html %}
The sum of the primes below 10 is $2 + 3 + 5 + 7 = 17$.

Find the sum of all the primes below two million.

## Solution
The [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) is an efficient algorithm in finding all primes up to a certain number $n$. Here is how the algorithm works:
1. Create a list of numbers $2, 3, \cdots, n$.
2. Let variable $p$ be 2, the smallest prime.
3. Cross out the multiples of $p$ in the list (excluding $p$ itself).
4. Find the smallest uncrossed number (i.e. a prime) that's greater than $p$.
    * If there is no such number, stop.
    * Otherwise, set $p$ to that value and go to step 3.
5. Return the list of uncrossed numbers. Those are all the primes below $n$.

There are many ways to optimise this algorithm:
* In step 3, note that all composite numbers below $p^2$ should have been crossed out by a previous smaller $p$. (Why?) Hence, we can start crossing out numbers from $p^2$ (instead of $2p$).
* All composite numbers below $n$ can be written in the form $ab$, where $1 \leq a < b$. It can be shown that $a \leq \sqrt{n}$. Therefore, any given composite number below $n$ would be crossed out by a prime number not greater than $\sqrt{n}$. We can terminate the loop early when $p > \sqrt{n}$. (Why??)
* Finding the next smallest uncrossed number at step 4 may take some time. When you think about it, we are trying to find the next prime in every iteration of step 4. Clearly we can skip even numbers to make the searching easier. Some programmers would also keep a large wheel of prime numbers to skip initial searching of the next uncrossed numbers.

Here is how I wrote the algorithm:
{% highlight python %}
def sieve_of_eratosthenes(n):
    """
    Finds all prime numbers under n.
    Returns the array a[0...n-1]. 
    If number i is prime, a[i] = 0.
    If number i is composite, a[i] contains one of i's non-trivial factors.
    """
    sieve = [0 for _ in range(n)]
    wheel = __wheel_factors()
    factor = next(wheel)
    while factor * factor < n:
        if sieve[factor] == 0:
            for i in range(factor * factor, n, factor):
                sieve[i] = factor
        factor = next(wheel)
    return sieve


def __wheel_factors():
    yield 2
    factor = 3
    while True:
        yield factor
        factor += 2
{% endhighlight %}