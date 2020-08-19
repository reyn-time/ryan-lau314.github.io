---
layout: post
title: "Problem 014: Longest Collatz Sequence"
date: 2020-08-19 09:31:02 +0100
tags: euler
---
{% include mathjax.html %}
## Problem 
The following iterative sequence is defined for the set of positive integers:

* $n \to n/2$ ($n$ is even)
* $n \to 3n + 1$ ($n$ is odd)

Using the rule above and starting with 13, we generate the following sequence:

\\\[13 \to 40 \to 20 \to 10 \to 5 \to 16 \to 8 \to 4 \to 2 \to 1\\\]

It can be seen that this sequence (starting at 13 and finishing at 1) contains 10 terms. Although it has not been proved yet (Collatz Problem), it is thought that all starting numbers finish at 1.

Which starting number, under one million, produces the longest chain?

NOTE: Once the chain starts the terms are allowed to go above one million.

## Solution
Define $f(x)$ to be the Collatz sequence length of $x$ for all $x \in \mathbb{N}$. The question can be seen as finding the solution to this expression:
\\\[\max\big(f(1), f(2), \cdots, f(999 999)\big)\\\]

From the definition of iterative sequence, it is not hard to spot that:
* $f(1) = 1$
* $f(n) = f(n/2) + 1$ (if $n$ is even)
* $f(n) = f(3n + 1) + 1$ (if $n$ is odd and not 1) 

Note that many chains overlap with each other. For example, within the $f(13)$ call (see above) we can see the whole Collatz chain of 40. We may be able to use [memoization](https://en.wikipedia.org/wiki/Memoization) here to save us some time when finding $f(40)$ later.

Here is how I've coded it in Python:
{% highlight python %}
memo = {}


def collatz_chain_length(n):
    # If cached, return cached value
    if n in memo:
        return memo[n]

    # Otherwise, compute value and cache it
    if n == 1:
        length = 1
    elif n % 2 == 0:
        length = collatz_chain_length(n // 2) + 1
    else:
        length = collatz_chain_length(3 * n + 1) + 1

    memo[n] = length
    return length


starting_number = 0
max_chain_length = 0
for i in range(1, 1_000_000):
    length = collatz_chain_length(i)
    if length > max_chain_length:
        starting_number, max_chain_length = (i, length)

print(f"Starting number: {starting_number}\nChain length: {max_chain_length}")
{% endhighlight %}