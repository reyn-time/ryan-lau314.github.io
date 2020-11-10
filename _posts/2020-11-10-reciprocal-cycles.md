---
layout: post
title: "Problem 026: Reciprocal Cycles"
date: 2020-11-10 20:18:33 +0100
tags: euler
---

{% include mathjax.html %}

## Problem

A unit fraction contains 1 in the numerator. The decimal representation of the unit fractions with denominators 2 to 10 are given:

- $1/2 = 0.5$
- $1/3 = 0.(3)$
- $1/4 = 0.25$
- $1/5 = 0.2$
- $1/6 = 0.1(6)$
- $1/7 = 0.(142857)$
- $1/8 = 0.125$
- $1/9 = 0.(1)$
- $1/10 = 0.1$

Where 0.1(6) means 0.166666..., and has a 1-digit recurring cycle. It can be seen that $1/7$ has a 6-digit recurring cycle.

Find the value of $d < 1000$ for which $1/d$ contains the longest recurring cycle in its decimal fraction part.

## Solution

The decimal representation of a unit fraction $1/d$ can be found by a long division algorithm:

```
function decimal_representation(d):
  remainder = 1
  while remainder > 0:
    digit = (10 * remainder) / d
    remainder = (10 * remainder) % d
    print(digit)
```

The function will continue to print out digits forever unless the decimal representation is terminating, i.e. the remainder is 0 at some point in time.

Let's try to dry run the algorithm with $d=7$:

| Iteration | Remainder (start) | d   | Digit      | Remainder (end) |
| --------- | ----------------- | --- | ---------- | --------------- |
| 1         | 1                 | 7   | 10 / 7 = 1 | 10 % 7 = 3      |
| 2         | 3                 | 7   | 30 / 7 = 4 | 30 % 7 = 2      |
| 3         | 2                 | 7   | 20 / 7 = 2 | 20 % 7 = 6      |
| 4         | 6                 | 7   | 60 / 7 = 8 | 60 % 7 = 4      |
| 5         | 4                 | 7   | 40 / 7 = 5 | 40 % 7 = 5      |
| 6         | 5                 | 7   | 50 / 7 = 7 | 50 % 7 = 1      |
| 7         | 1                 | 7   | 10 / 7 = 1 | 10 % 7 = 3      |

At iteration 7, we have the same remainder as that in iteration 1 initially. Therefore, we can deduce that $1/7$ will have a non-terminating decimal sequence $(142857)$ that repeats itself every 6 digits.

Similarly, if the first two occurences of a repeated remainder appear on iteration $i$ and $j$ respectively, we can tell that the recurring cycle length of the unit fraction is $j - i$.

Here is how I wrote the algorithm in Python:
{% highlight python %}
def cycle_length(n: int) -> int:
    """
    Returns the length of the recurring cycle in the decimal representation of 1/n.

    If there is no recurring cycle, returns 0.
    """
    remainder: int = 10 % n
    counter: int = 1
    previousRemainders: Dict[int, int] = {1: 0}

    while remainder != 0 and remainder not in previousRemainders:
        previousRemainders[remainder] = counter
        counter += 1
        remainder = 10 * remainder % n

    if remainder == 0:
        return 0

    return counter - previousRemainders[remainder]

{% endhighlight %}

The longest cycle length for the unit fractions $\\\{1/1, \cdots ,1/999\\\}$ is 982. It is surprisingly large IMO.