---
layout: post
title: "Problem 009: Special Pythagorean Triplet"
date: 2020-08-04 22:00:10 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
A Pythagorean triplet is a set of three natural numbers, $a < b < c$, for which,

\\\[a^2 + b^2 = c^2\\\]
For example, $3^2 + 4^2 = 9 + 16 = 25 = 5^2$.

There exists exactly one Pythagorean triplet for which $a + b + c = 1000$.

Find the product $abc$.

## Solution
Not a very challenging problem. I will just paste my solution here.
{% highlight python %}
for a in range(1, 1000):
    for b in range(a + 1, 1000):
        c = 1000 - a - b
        if c > b and a * a + b * b == c * c:
            print(f"{a} * {b} * {c} = {a * b * c}")
{% endhighlight %}

It takes at most $999 + 998 + \cdots + 1 = 1000 * 999 \div 2 = 499500$ iterations to find a solution. Not a lot of work for a computer that can run millions of instructions per second.

There are a few places for optimization. For example:
* If $c \leq b$, later iterations of the inner loop will not lead to a situation where $c > b$. We can terminate the inner loop early.
* If $a^2 + b^2 > c^2$, increasing the value of $b$ will not make $a^2+b^2=c^2$. Hence, we can stop the inner loop once this condition is reached.
* We can exit the code early when a solution is found.

The optimised code looks like this: (Not that it needs more optimisation though...)
{% highlight python %}
for a in range(1, 1000):
    for b in range(a + 1, 1000):
        c = 1000 - a - b
        if c <= b:
            break
        if a * a + b * b > c * c:
            break
        if a * a + b * b == c * c:
            print(f"{a} * {b} * {c} = {a * b * c}")
            exit(0)
{% endhighlight %}