---
layout: post
title: "Problem 030: Digit Fifth Powers"
date: 2020-11-12 19:47:51 +0000
tags: euler
---

{% include mathjax.html %}

## Problem

Surprisingly there are only three numbers that can be written as the sum of fourth powers of their digits:

- $1634 = 1^4 + 6^4 + 3^4 + 4^4$
- $8208 = 8^4 + 2^4 + 0^4 + 8^4$
- $9474 = 9^4 + 4^4 + 7^4 + 4^4$

As $1 = 1^4$ is not a sum it is not included.

The sum of these numbers is 1634 + 8208 + 9474 = 19316.

Find the sum of all the numbers that can be written as the sum of fifth powers of their digits.

## Solution

It is rather easy to determine if a number can be written as the sum of fifth powers:
{% highlight python %}
def sum_of_fifth_powers_of_digits(n):
    total = 0
    while n > 0:
        last_digit = n % 10
        total += last_digit ** 5
        n //= 10
    return total

def is_sum_of_fifth_powers(n):
    return n == sum_of_fifth_powers_of_digits(n)
{% endhighlight %}

But we cannot run this algorithm on infinitely many integers! The search space must be limited.

It would be useful to prove the following statement:
> If the number has more than 6 digits, the number cannot be written as a sum of fifth powers.

Consider arbitrary $N$ with $n > 6$ digits.

Let $N = \overline{d_0d_1 \cdots d_{n-1}}$, where $d_0, \cdots, d_{n-1}$ are the digits of the number $n$.

Since $N$ has $n$ digits, $N \geq 10^{n-1}$. 

However, 
\\\[d_0d_1 \cdots d_{n-1} \leq \overbrace{9 \cdot 9 \cdot\cdots\cdot 9}^n = 9^n \\\]

For all $n > 6$, $9^n < 10^{n-1}$. Therefore $d_0d_1 \cdots d_{n-1} \not= N$.

By the statement above, we could limit the search space to $[10, 10^6)$. 