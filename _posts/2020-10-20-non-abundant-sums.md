---
layout: post
title: "Problem 023: Non-abundant Sums"
date: 2020-10-20 21:03:48 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
A perfect number is a number for which the sum of its proper divisors is exactly equal to the number. For example, the sum of the proper divisors of 28 would be $1 + 2 + 4 + 7 + 14 = 28$, which means that 28 is a perfect number.

A number $n$ is called deficient if the sum of its proper divisors is less than $n$ and it is called abundant if this sum exceeds $n$.

As 12 is the smallest abundant number, $1 + 2 + 3 + 4 + 6 = 16$, the smallest number that can be written as the sum of two abundant numbers is 24. By mathematical analysis, it can be shown that all integers greater than 28123 can be written as the sum of two abundant numbers. However, this upper limit cannot be reduced any further by analysis even though it is known that the greatest number that cannot be expressed as the sum of two abundant numbers is less than this limit.

Find the sum of all the positive integers which cannot be written as the sum of two abundant numbers.
## Solution
To determine if a number is abundant, I've used the `divisors` method created [a few posts ago]({% post_url 2020-10-19-amicable-numbers %}): 
{% highlight python %}
def is_abundant(n):
    return sum(divisors(n)) - n > n
{% endhighlight %}

It is not very easy to find all non-abundant sums. However, through exhaustion we can easily find the set of all abundant sums not greater than 28123:
{% highlight python %}
abundant_numbers = [i for i in range(1, 28124) if is_abundant(i)]

abundant_sums = set()
for i in abundant_numbers:
    for j in abundant_numbers:
        if i + j <= 28123 and i + j not in abundant_sums:
            abundant_sums.add(i + j)
{% endhighlight %}

It is then trivial to the sum of all non-abundant sums:
{% highlight python %}
non_abundant_sums = [i for i in range(1, 28124) if i not in abundant_sums]
print(sum(non_abundant_sums))
{% endhighlight %}

There is probably a more clever solution, but brute-force doesn't take long anyway.