---
layout: post
title: "Problem 021: Amicable numbers"
date: 2020-10-19 21:04:00 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
Let $d(n)$ be defined as the sum of proper divisors of $n$ (numbers less than $n$ which divide evenly into $n$).
If $d(a) = b$ and $d(b) = a$, where $a \not= b$, then $a$ and $b$ are an amicable pair and each of $a$ and $b$ are called amicable numbers.

For example, the proper divisors of 220 are 1, 2, 4, 5, 10, 11, 20, 22, 44, 55 and 110; therefore $d(220) = 284$. The proper divisors of 284 are 1, 2, 4, 71 and 142; so $d(284) = 220$.

Evaluate the sum of all the amicable numbers under 10000.
## Solution
The hard part of this problem is to find all the divisors of a number $n$. With some simple tricks, it is easy to find all of them within $O(\sqrt{n})$ time:
{% highlight python %}
def divisors(n):
    divisors_list = []

    i = 1
    while i * i <= n:
        if n % i == 0:
            divisors_list.append(i)
            if i * i != n:
                divisors_list.append(n // i)
        i += 1

    return divisors_list
{% endhighlight %} 
With this function ready, it is not hard to exhaust through all pairs of numbers $(a, b)$ under 10000 where $d(a) = b$ and $d(b) = a$. Just note that $(a, b)$ and $(b, a)$ are considered to be the same pair.

Here's how I've written the complete search code. I have only counted the pairs of $(a, b)$ where $a < b$ to avoid double-counting.
{% highlight python %}
def d(n):
    return sum(divisors(n)) - n

total = 0
for a in range(1, 10000):
    b = d(a)
    if a < b and d(b) == a:
        print(f"Pair found: ({a}, {b})")
        total += a + b

print(total)
{% endhighlight %}