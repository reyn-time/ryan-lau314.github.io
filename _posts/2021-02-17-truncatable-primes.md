---
layout: post
title: "Problem 037: Truncatable Primes"
date: 2021-02-17 21:45:30 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The number 3797 has an interesting property. Being prime itself, it is possible to continuously remove digits from left to right, and remain prime at each stage: 3797, 797, 97, and 7. Similarly we can work from right to left: 3797, 379, 37, and 3.

Find the sum of the only eleven primes that are both truncatable from left to right and right to left.

NOTE: 2, 3, 5, and 7 are not considered to be truncatable primes.

## Solution
A prime $p = d_1 \mathbin\Vert d_2 \mathbin\Vert \cdots \mathbin\Vert d_n$ is truncatable if it satisfies two properties:
* Left-truncatable: If we remove any prefix of the prime, the remaining number is still a prime. Mathematically speaking, we need the following to hold:
\\\[ LT(p) \equiv \forall 2 \leq i \leq n.\; prime(d_i \mathbin\Vert d_{i+1} \mathbin\Vert \cdots \mathbin\Vert d_n) \\\]

* Right-truncatable: If we remove any suffix of the prime, the remaining number is still a prime. The maths is more or less the same:
\\\[  RT(p) \equiv \forall 1 \leq i < n.\; prime(d_1 \mathbin\Vert d_2 \mathbin\Vert \cdots \mathbin\Vert d_i) \\\]

My main flow goes like this:
{% highlight python %}
solution = []
prime_gen = primes()

while len(solution) < 11:
    prime = next(prime_gen)
    left = is_left_truncatable(prime)
    right = is_right_truncatable(prime)
    if left and right and prime > 10:
        solution.append(prime)

print(solution)
print(sum(solution))
{% endhighlight %}

For the code to work I implemented a prime generator... with the help of Stack Overflow.
I wrote some documentation for clarity.
{% highlight python %}
def primes():
    """
    Slightly efficient prime generator.
    """
    # (x, D[x]): x is a composite number, with its smallest prime = D[x].
    D = {}
    yield 2
    # q in [3, 5, 7, ...]
    for q in islice(count(3), 0, None, 2):
        p = D.pop(q, None)
        if p is None:
            # Cannot find q as a key: q is prime.
            # Record (q^2, q) in the dictionary.
            D[q * q] = q
            yield q
        else:
            # Can find q as a key: q is composite with smallest prime p.
            # Find smallest n such that (q + 2np) is not yet recorded in dictionary.
            # Record (q + 2np, p) in the dictionary.
            x = q + 2 * p
            while x in D:
                x += 2 * p
            D[x] = p
{% endhighlight %}

There is still a remaining problem - how do we check for left/right-truncatability?
Note that we could actually define $RT(p)$ in a recursive way.
\\\[RT(p) = \begin{cases} prime(p)&(p<10) \\\\ prime(p) \wedge RT\left(\left\lfloor \frac{p}{10} \right\rfloor\right) & (p \geq 10) \end{cases} \\\]

{% highlight python %}
def is_right_truncatable(n):
   return is_prime(n) and (n < 10 or is_right_truncatable(n // 10))
{% endhighlight %}

A similar function `is_left_truncatable` can be written recursively.