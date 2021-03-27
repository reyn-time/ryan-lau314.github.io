---
layout: post
title: "Problem 046: Goldbach's Other Conjecture"
date: 2021-03-24 21:50:15 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
It was proposed by Christian Goldbach that every odd composite number can be written as the sum of a prime and twice a square.

- $9 = 7 + 2\times 1^2$
- $15 = 7 + 2\times 2^2$
- $21 = 3 + 2\times 3^2$
- $25 = 7 + 2\times 3^2$
- $27 = 19 + 2\times 2^2$
- $33 = 31 + 2\times 1^2$

It turns out that the conjecture was false.

What is the smallest odd composite that cannot be written as the sum of a prime and twice a square?
## Solution
We will need to solve two different subproblems:
- How to generate an infinite list of composite numbers?
- How to check if a given composite number satisfies Goldbach's conjecture?

The first subproblem can be solved easily by using the [`sieve` method](https://docs.sympy.org/latest/modules/ntheory.html#sympy.ntheory.generate.Sieve) in the `sympy` library. (Yes I am being lazy...) 

We just need to iterate through all the odd numbers above 9, and output the ones that are composite, as shown below:

{% highlight python %}
from sympy import sieve
from itertools import count

def odd_composite_generator():
    for n in count(9, 2):
        if n not in sieve:
            yield n
{% endhighlight %}

The next problem is slightly trickier. To prove a number $n$ satisfies the conjecture, we need to find a pair of numbers $(p, i)$ such that $n = p + 2i^2$. 
For a given $n$, I exhaust all possible values of $i$ and check if $p' = n - 2i^2$ is a prime. If all values of $i$ cannot make $p'$ prime, the number $n$ cannot satisfy the conjecture.

The following function returns the pair of numbers $(p, i)$ if the number can satisfy the conjecture. Otherwise `None` is returned.

{% highlight python %}
def satisfies_goldbach(n):
    i = 1
    while n > 2 * i * i:
        maybe_prime = n - 2 * i * i
        if maybe_prime in sieve:
            return (maybe_prime, i)
        i += 1
    return None
{% endhighlight %}

We can solve the problem by combining the two functions together:

{% highlight python %}
gen = odd_composite_generator()
while True:
    odd_composite = next(gen)
    tuple = satisfies_goldbach(odd_composite)
    if tuple:
        prime, i = tuple
        print(f"{odd_composite} = {prime} + 2 * {i}^2")
    else:
        print(f"{odd_composite} does not satisfy conjecture!")
        break
{% endhighlight %}