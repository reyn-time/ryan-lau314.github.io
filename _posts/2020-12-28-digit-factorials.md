---
layout: post
title: "Problem 034: Digit Factorials"
date: 2020-12-28 16:28:55 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
145 is a curious number, as $1! + 4! + 5! = 1 + 24 + 120 = 145$.

Find the sum of all numbers which are equal to the sum of the factorial of their digits.

Note: As $1! = 1$ and $2! = 2$ are not sums they are not included.

## Solution
A problem very similar to the [Digit Fifth Powers]({% post_url 2020-11-12-digit-fifth-powers %}) one.

The sum of factorial of the digits of a number can be computed quite easily:
{% highlight python %}
import math 

def sum_of_factorial_of_digits(n):
    sum = 0
    while n > 0:
        digit = n % 10
        sum += math.factorial(digit)
        n //= 10
    return sum
{% endhighlight %}

To make sure our code terminates, we need to limit our search scope! Thankfully, we can prove that a number with such property could only have at most 7 digits. We can therefore limit the search space to $[10, 10^7)$.

### Proof
Assume the contrary, there exists a number $N = d_1 \mathbin\Vert d_2 \mathbin\Vert \cdots \mathbin\Vert d_n$, where the number of digits $n \geq 8$. 
\\\[d_1! + d_2! + \cdots + d_n! \leq \overbrace{9! + \cdots + 9!}^n = 362880n \\\]

For $n \geq 8$,
\\\[362880n < 10^{n-1} \leq N\\\]

Therefore $d_1! + d_2! + \cdots + d_n! \not= N$. Contradiction is found.