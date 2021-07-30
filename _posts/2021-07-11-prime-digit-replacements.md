---
layout: post
title: "Problem 051: Prime Digit Replacements"
date: 2021-07-11 16:53:55 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
By replacing the 1<sup>st</sup> digit of the 2-digit number *3, it turns out that six of the nine possible values: 13, 23, 43, 53, 73, and 83, are all prime.

By replacing the 3<sup>rd</sup> and 4<sup>th</sup> digits of 56**3 with the same digit, this 5-digit number is the first example having seven primes among the ten generated numbers, yielding the family: 56003, 56113, 56333, 56443, 56663, 56773, and 56993. Consequently 56003, being the first member of this family, is the smallest prime with this property.

Find the smallest prime which, by replacing part of the number (not necessarily adjacent digits) with the same digit, is part of an eight prime value family.

## Solution

Took me quite a lot of time to come up with the solution...

I tried exhausting all different type of masks: `*`, `**`, `*0`, `*1`, ... For a mask of length $n$, say $m_1m_2\cdots m_n$, each mask digit can take 11 different values. This implies there are $11^n$ different types of mask. We can go through all those masks, but it is very very slow.

Instead, let's try to consider all primes with $n$ digits (say $n=2$). Take one of the 2-digit prime, like 23. You can see it belongs to two masks only: `2*`, `*3`.

By finding the masks for each prime, we can find which mask is most popular:

- **: [11]
- *1: [11, 31, 41, 61, 71]
- 1*: [11, 13, 17, 19]
- *3: [13, 23, 43, 53, 73, 83]
- *7: [17, 37, 47, 67, 97]
- *9: [19, 29, 59, 79, 89]
- 2*: [23, 29]
- 3*: [31, 37]
- 4*: [41, 43, 47]
- 5*: [53, 59]
- 6*: [61, 67]
- 7*: [71, 73, 79]
- 8*: [83, 89]
- 9*: [97]

Each prime does not have many associated masks. In the above example we only encountered 14 distinct masks, much less than $11^2 = 121$. You can see how this greatly limits the search space. For 2-digit masks, `*3` holds the largest family of primes. We can do similar work for larger $n$'s.

By [prime number theorem](https://en.wikipedia.org/wiki/Prime_number_theorem), as $n$ increases, primes becomes much rarer. As long as we have an efficient prime generator, this strategy should give us a quicker solution.

I used backtracking to get all possible masks of a prime:
{% highlight python %}
def mask_prime(prime):
    masks = []
    for i in range(10):
        mask_prime_helper(str(prime), "", chr(ord("0") + i), masks)
    return masks


def mask_prime_helper(prime, mask, digit, masks):
    if len(prime) == len(mask):
        if prime != mask:
            masks += [mask]
    else:
        i = len(mask)
        c = prime[i]
        if c == digit:
            mask_prime_helper(prime, mask + "*", digit, masks)
        mask_prime_helper(prime, mask + c, digit, masks)
{% endhighlight %}

Once we have the method `mask_prime` ready, it is not hard to write the rest of the algorithm. The code below retrieves the mask with the largest prime family, given the number of digits in the mask. The `sieve` function helped generating all necessary primes.

{% highlight python %}
from sympy import sieve

def largest_prime_family_with_num_of_digits(num_of_digits):
    mask_to_primes = dict()
    for prime in primes_with_num_of_digits(num_of_digits):
        for mask in mask_prime(prime):
            if mask not in mask_to_primes:
                mask_to_primes[mask] = []
            mask_to_primes[mask].append(prime)

    largest_family_key = ""
    largest_family_values = []
    for family_key, family_values in mask_to_primes.items():
        if len(family_values) > len(largest_family_values) or (
            len(family_values) == len(largest_family_values)
            and family_values < largest_family_values
        ):
            largest_family_key = family_key
            largest_family_values = family_values

    return largest_family_key, largest_family_values


def primes_with_num_of_digits(num_of_digits):
    return [n for n in sieve.primerange(10 ** (num_of_digits - 1), 10 ** num_of_digits)]
{% endhighlight %}
