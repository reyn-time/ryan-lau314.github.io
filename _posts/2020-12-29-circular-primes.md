---
layout: post
title: "Problem 035: Circular Primes"
date: 2020-12-29 12:24:39 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The number, 197, is called a circular prime because all rotations of the digits: 197, 971, and 719, are themselves prime.

There are thirteen such primes below 100: 2, 3, 5, 7, 11, 13, 17, 31, 37, 71, 73, 79, and 97.

How many circular primes are there below one million?

## Solution
Define $rot: \mathbb{N} \to \mathbb{N}$ to be the function that satisfies
\\\[ rot(d_1  \mathbin\Vert d_2 \mathbin\Vert \cdots \mathbin\Vert d_n) = d_n \mathbin\Vert d_1 \mathbin\Vert \cdots \mathbin\Vert d_{n-1} \\\]
for all natural numbers $N = d_1 \mathbin\Vert d_2 \mathbin\Vert\cdots\mathbin\Vert d_n$.

We can determine if a prime $N$ is circular with the following algorithm:
* $C \leftarrow rot(N)$
* While $C \not= N$:
  * If $C$ is not prime:
    * Return False
  * Otherwise:
    * $C \leftarrow rot(C)$
* Return True

How do we know if $C$ is prime though? Since we are only searching for circular primes below one million, a way is to precompute the set of primes under one million, say $S$, and check if $C \in S$. The precomputation can be done easily by the [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes).

Here's how I've solved the problem in Python. The part related to the Sieve of Eratosthenes (`primes_within_range`) is omitted for simplicity.
{% highlight python %}
def rotate(number):
    number_string = str(number)
    return int(number_string[-1] + number_string[:-1])

def is_circular_prime(prime, primes):
    next_rotation = rotate(prime)
    while next_rotation != prime:
        if next_rotation not in primes:
            return False
        next_rotation = rotate(next_rotation)
    return True

primes = set([prime for prime in primes_within_range(1_000_000)])
circular_primes = [prime for prime in primes if is_circular_prime(prime, primes)]
print(len(circular_primes))
{% endhighlight %}