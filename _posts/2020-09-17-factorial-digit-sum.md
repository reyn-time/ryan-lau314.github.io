---
layout: post
title: "Problem 020: Factorial Digit Sum"
date: 2020-09-17 19:13:21 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
$n!$ means $n \times (n − 1) \times \cdots \times 3 \times 2 \times 1$

For example, $10! = 10 \times 9 \times \cdots \times 3 \times 2 \times 1 = 3628800$,
and the sum of the digits in the number $10!$ is $3 + 6 + 2 + 8 + 8 + 0 + 0 = 27$.

Find the sum of the digits in the number $100!$

## Solution
Similar to [Problem 016]({% post_url 2020-09-02-power-digit-sum %}), this problem requires programmers to compute a large number without overflow. It is quite straightforward to do this in Python, so I will just paste my solution here:
{% highlight python %}
import math

def digit_sum(n):
    sum = 0
    while n > 0:
        sum += n % 10
        n //= 10
    return sum

print(digit_sum(math.factorial(100)))
{% endhighlight %}