---
layout: post
title: "Problem 005: Smallest Multiple"
date: 2020-07-28 09:47:32 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
2520 is the smallest number that can be divided by each of the numbers from 1 to 10 without any remainder.

What is the smallest positive number that is evenly divisible by all of the numbers from 1 to 20?

## Solution
The question is essentially asking for the least common multiple (LCM) of $1, 2, \cdots, 20$. 

It is known that for all pairs of $x, y \in \mathbb{N}$:
\\\[ \operatorname{lcm}(x, y) = \frac{xy}{\gcd(x, y)} \\\]

Thankfully, there is a [very efficient algorithm](https://en.wikipedia.org/wiki/Euclidean_algorithm#Procedure) that computes the value of $\gcd(x, y)$ in $\mathcal{O}(\log(\min(x, y)))$ time. The algorithm is shown below:

{% highlight python %}
def gcd(x, y):
    if y == 0:
        return x
    return gcd(y, x % y)
{% endhighlight %}

Using reducers and the algorithm above, it is not hard to compute the LCM for an arbitary list of numbers:
{% highlight python %}
def lcm(*vals):
    return reduce(__lcm, vals)

def __lcm(x, y):
    return x * y // gcd(x, y)

def gcd(x, y):
    if y == 0:
        return x
    return gcd(y, x % y)
{% endhighlight %}
