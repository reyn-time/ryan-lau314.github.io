---
layout: post
title: "Problem 004: Largest Palindrome Product"
date: 2020-07-28 09:24:10 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
A palindromic number reads the same both ways. The largest palindrome made from the product of two 2-digit numbers is $9009 = 91 \times 99$.

Find the largest palindrome made from the product of two 3-digit numbers.

## Solution
The simplest approach is to go through all pairs of $(i, j)$'s, where $100 \leq i, j < 1000$. Computer is quite fast, so we can find the largest palindromic $ij$ in no time. 

Here is a sample code:

{% highlight python %}
def is_palindrome(n):
    return str(n) == str(n)[::-1]

max_palindrome = 0
for i in range(100, 1000):
    for j in range(100, 1000):
        product = i * j

        if is_palindrome(product) and product > max_palindrome:
            max_palindrome = product

print(max_palindrome)
{% endhighlight %}

There is some room for optimization. For example, if we have gone through the pair $(i, j) = (100, 200)$, there is no need to try out the pair $(i, j) = (200, 100)$ because they give the exact same product. We can reduce our search space by only checking those $i$, $j$'s where $i \leq j$. (Why?)

To optimize even further, we should be searching in a descending order. Suppose we have found a palindrome at $(i, j) = (I, J)$ for some constant $I$, $J$. Then it is no longer necessary to try out the list of $(i, j)$'s where: 
* $i = I$ and $j < J$. (Why?)
* $ij < IJ$ (Why?) 

Here's how I've written the optimized code in Python:
{% highlight python %}
max_palindrome = 0
for i in reversed(range(100, 1000)):
    for j in reversed(range(i + 1, 1000)):
        product = i * j

        if product < max_palindrome:
            break

        if is_palindrome(product) and product > max_palindrome:
            max_palindrome = product

print(max_palindrome)
{% endhighlight %}
