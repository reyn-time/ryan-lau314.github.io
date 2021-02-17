---
layout: post
title: "Problem 036: Double-base Palindromes"
date: 2021-02-17 21:31:11 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The decimal number, $585 = 1001001001_2$ (binary), is palindromic in both bases.

Find the sum of all numbers, less than one million, which are palindromic in base 10 and base 2.

(Please note that the palindromic number, in either base, may not include leading zeros.)

## Solution
I re-used the [is_palindrome]({% post_url 2020-07-28-largest-palindrome-product %}) function from a previous problem.
{% highlight python %}
def is_palindrome(n):
    """
    Tests if the input is a palindrome.
    Input can be either a number or a string.
    """
    if isinstance(n, int):
        return __is_palindrome(str(n))
    return __is_palindrome(n)


def __is_palindrome(s):
    return s == s[::-1]
{% endhighlight %}

Note the funky syntax `s[::-1]`. It reverses the string `s`! Probably only something that you can do in Python?

The only problem left, then, is to convert a base 10 number to a base 2 string. Thankfully Python provides a built-in function [bin](https://docs.python.org/3/library/functions.html#bin) exactly for this purpose. I just need to remove the prefix `0b` from the output:

{% highlight python %}
def base10_to_base2(base10):
    return bin(base10)[2:]
{% endhighlight %}

The solution can then be written as a two-liner. I guess you can shrink it down to one line if you try really hard...
{% highlight python %}
solution = sum([number for number in range(1, 1_000_000) if is_palindrome(number) and is_palindrome(base10_to_base2(number))])
print(solution)
{% endhighlight %}