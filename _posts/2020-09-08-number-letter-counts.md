---
layout: post
title: "Problem 017: Number Letter Counts"
date: 2020-09-08 20:56:54 +0100
tags: euler
---

{% include mathjax.html %}
## Problem 
If the numbers 1 to 5 are written out in words: one, two, three, four, five, then there are $3 + 3 + 5 + 4 + 4 = 19$ letters used in total.

If all the numbers from 1 to 1000 (one thousand) inclusive were written out in words, how many letters would be used?

NOTE: Do not count spaces or hyphens. For example, 342 (three hundred and forty-two) contains 23 letters and 115 (one hundred and fifteen) contains 20 letters. The use of "and" when writing out numbers is in compliance with British usage.

## Solution
One may come up with such naive pseudocode for spelling out number $n$ below 100:
* Let array $A = ['zero', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']$.
* If $n < 10$:
    * Print '$A[n]$'.
* Else if $11 \leq n \leq 19$:
    * Print '$A[n]$-teen'.
* Else if $n$ is a multiple of 10:
    * Print '$A[n / 10]$-ty'. 
* Else:
    * Print '$A[n / 10]$-ty $A[n \\% 10]$'.

Although this may generally work, there are quite a few special cases. For example, 11 (eleven) is not 'oneteen', 18 (eighteen) is not 'eightteen', and 20 (twenty) is not 'twoty'. Care must be taken to handle all of these cases.

Here is part of the code that I have written to solve the problem. It is quite tedious to write.

{% highlight python %}
DICT = {
    1: "one",
    2: "two",
    3: "three",
    4: "four",
    5: "five",
    6: "six",
    7: "seven",
    8: "eight",
    9: "nine",
    10: "ten",
    11: "eleven",
    12: "twelve",
    13: "thirteen",
    15: "fifteen",
    18: "eighteen",
    20: "twenty",
    30: "thirty",
    40: "forty",
    50: "fifty",
    80: "eighty",
}


def two_digit_to_words(n):
    # Special case: 1~13
    if n in DICT:
        return [DICT[n]]

    # Special case: 14~19
    if n < 20:
        return [DICT[n - 10] + "teen"]

    words = []
    tens = n // 10 * 10
    if tens in DICT:
        # Special tens: e.g. twenty, thirty
        words.append(DICT[tens])
    else:
        # Generic tens: e.g. sixty, seventy
        words.append(DICT[n // 10] + "ty")

    # Add ones for those that are not multiples of ten
    if n % 10 != 0:
        words += [DICT[n % 10]]

    return words


def word_length(words):
    return sum([len(word) for word in words])
{% endhighlight %}