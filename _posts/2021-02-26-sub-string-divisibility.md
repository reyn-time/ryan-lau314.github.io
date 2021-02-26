---
layout: post
title: "Problem 043: Sub-string Divisibility"
date: 2021-02-26 19:09:11 +0100
tags: euler
---

{% include mathjax.html %}

## Problem

The number, 1406357289, is a 0 to 9 pandigital number because it is made up of each of the digits 0 to 9 in some order, but it also has a rather interesting sub-string divisibility property.

Let $d_1$ be the 1<sup>st</sup> digit, $d_2$ be the 2<sup>nd</sup> digit, and so on. In this way, we note the following:

- $d_2d_3d_4=406$ is divisible by 2
- $d_3d_4d_5=063$ is divisible by 3
- $d_4d_5d_6=635$ is divisible by 5
- $d_5d_6d_7=357$ is divisible by 7
- $d_6d_7d_8=572$ is divisible by 11
- $d_7d_8d_9=728$ is divisible by 13
- $d_8d_9d_{10}=289$ is divisible by 17

Find the sum of all 0 to 9 pandigital numbers with this property.

## Solution

Though it is not hard to exhaust through all possible 0 to 9 pandigital numbers, I thought it would be nice to introduce a [backtracking](https://en.wikipedia.org/wiki/Backtracking) solution here. If we are only looking for numbers where $d_2d_3d_4$ is even, why bother enumerating candidates with the prefix "1235"...? By terminating earlier, we save a lot of precious computation time.

{% highlight python %}
def find_pandigital_numbers():
    pandigital_numbers = []
    backtrack(0, 0, [True for _ in range(10)], pandigital_numbers)
    return pandigital_numbers

'''
Finds all pandigital numbers with the sub-string divisibility property.

:param level: Length of the current number.
:param number: Current number. It is a prefix of a candidate pandigital number.  
:param have_digit: An array of digits. have_digit[i] is True iff digit i is within the current number.
:param pandigital_numbers: The current list of pandigital numbers with the specific property.
'''
def backtrack(level, number, have_digit, pandigital_numbers):
    if level >= 10:
        # 10 digits in number. Passed all divisibility tests.
        # Append it to the list of special pandigital numbers.
        pandigital_numbers.append(number)
        return

    for i in range(10):
        # Append 0~9 to the back of the current number.
        next_number = number * 10 + i

        # If still pandigital and satisfy divisibility properties:
        if have_digit[i] and is_substring_divisible(level, next_number):
            # Prepare to exhaust deeper!
            have_digit[i] = False
            backtrack(level + 1, next_number, have_digit, pandigital_numbers)
            have_digit[i] = True


PRIMES = [2, 3, 5, 7, 11, 13, 17]
def is_substring_divisible(level, number):
    return level < 3 or number % 1000 % PRIMES[level - 3] == 0
{% endhighlight %}

Backtracking is quite hard to understand, so I added some documentation to the helper method `backtrack`.

The list of pandigital numbers with the substring divisibility property can be found by calling `find_pandigital_numbers()`.