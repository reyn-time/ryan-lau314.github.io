---
layout: post
title: "Problem 016: Power Digit Sum"
date: 2020-09-02 20:56:07 +0100
tags: euler
---
{% include mathjax.html %}
## Problem 
$2^{15} = 32768$ and the sum of its digits is $3 + 2 + 7 + 6 + 8 = 26$.

What is the sum of the digits of the number $2^{1000}$?

## Solution
The hard part of this problem is to compute $2^{1000}$ without overflow. Since Python supports arbitrary-precision integers, I didn't have to write a lot of code.

{% highlight python %}
def digit_sum(n):
    sum = 0
    while n > 0:
        sum += n % 10
        n //= 10
    return sum

print(digit_sum(2 ** 1000))
{% endhighlight %}