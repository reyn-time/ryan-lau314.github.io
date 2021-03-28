---
layout: post
title: "Problem 049: Prime Permutations"
date: 2021-03-28 15:03:10 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The arithmetic sequence, 1487, 4817, 8147, in which each of the terms increases by 3330, is unusual in two ways: (i) each of the three terms are prime, and, (ii) each of the 4-digit numbers are permutations of one another.

There are no arithmetic sequences made up of three 1-, 2-, or 3-digit primes, exhibiting this property, but there is one other 4-digit increasing sequence.

What 12-digit number do you form by concatenating the three terms in this sequence?

## Solution
I splitted the problem into three subproblems:
1. Get all primes between 1000 and 9999.
1. Split the primes into disjoint sublists. All primes in each sublist are permutations of each other.
1. Find all arithemetic subsequences in each sublist.

The first subproblem is relatively simple. Again I use the [`primerange` function](https://docs.sympy.org/latest/modules/ntheory.html#sympy.ntheory.generate.Sieve.primerange) from the good old `sympy` library. I love you sympy.

The second subproblem, grouping primes, needs a bit more thought. I did so by defining a key $k(p)$ for each prime $p$. Each prime is keyed by its digits in ascending order. For example, the prime 9629 will have the key 2699. The task can then be completed by grouping the primes according to their keys:

{% highlight python %}
def group_prime_permutations():
    prime_dict = {}

    for prime in sieve.primerange(1000, 9999):
        prime_key = key(prime)
        prime_dict.setdefault(prime_key, [])
        prime_dict[prime_key].append(prime)

    return prime_dict

def key(prime):
    return "".join(sorted(str(prime)))
{% endhighlight %}

The first few entries of the dictionary look like this:
```
{
    '0019': [1009, 9001], 
    '0113': [1013, 1031, 1103, 1301, 3011], 
    '0119': [1019, 1091, 1109, 1901, 9011],
    ...
}
```

The third step is not too hard. For each sublist $L_i$ we just need to exhaust through all pairs of numbers $n, m \in L_i$, and see if the third number in the arithmetic sequence, i.e. $m + (m - n) = 2m - n$ exists in $L_i$. 

The following code demonstrates how the concept can be implemented. I removed some of my optimisations for clarity.

{% highlight python %}
def arithmetic_subsequence(primes):
    n = len(primes)

    for i in range(n):
        for j in range(i + 1, n):
            first_prime = primes[i]
            second_prime = primes[j]
            possible_prime = second_prime * 2 - first_prime

            if possible_prime in primes:
                yield (first_prime, second_prime, possible_prime)
{% endhighlight %}

By grouping the three steps together we obtain the complete solution:

{% highlight python %}
def p049():
    prime_dict = group_prime_permutations()
    print(prime_dict)

    for primes in prime_dict.values():
        for sequence in arithmetic_subsequence(primes):
            print(sequence)
{% endhighlight %}