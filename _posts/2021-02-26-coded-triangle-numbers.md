---
layout: post
title: "Problem 042: Coded Triangle Numbers"
date: 2021-02-26 08:55:25 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The n<sup>th</sup> term of the sequence of triangle numbers is given by, $t_n = \frac{1}{2}n(n+1)$; so the first ten triangle numbers are:

<p style="text-align:center">1, 3, 6, 10, 15, 21, 28, 36, 45, 55, ...</p>

By converting each letter in a word to a number corresponding to its alphabetical position and adding these values we form a word value. For example, the word value for SKY is $19 + 11 + 25 = 55 = t_{10}$. If the word value is a triangle number then we shall call the word a triangle word.

Using [words.txt](https://projecteuler.net/project/resources/p042_words.txt) (right click and 'Save Link/Target As...'), a 16K text file containing nearly two-thousand common English words, how many are triangle words?

## Solution
Previously we did [a question that computes the value of a word]({% post_url 2020-10-19-names-scores %}). It is not too hard I think.

The only remaining problem is to check if the word value is a triangle number. For now let's focus on a similar subproblem:
> Express index n in terms of its corresponding triangle number t<sub>n</sub>.

This can be done by a technique known as [completing the square](https://en.wikipedia.org/wiki/Completing_the_square):
\\\[\begin{align}
t_n &= \frac{1}{2}n(n+1)\\\\\
2t_n &= n^2 + n\\\\\
2t_n + \frac{1}{4} &= n^2 + n + \frac{1}{4}\\\\\
2t_n + \frac{1}{4} &= \left(n + \frac{1}{2}\right)^2\\\\\
8t_n + 1 &= (2n + 1)^2\\\\\
\sqrt{8t_n + 1} &= 2n + 1 \tag{1}\label{a} \\\\\
n &= \frac{1}{2}\left(\sqrt{8t_n + 1} - 1\right)
\end{align}\\\]

Note \ref{a}: I omitted the case where $ - \sqrt{8t_n + 1} = 2n + 1$. That's impossible since $2n + 1 > 0$ for all $n$. 

From the last equation, we can deduce something very useful: $w$ is a triangle number if and only if $\frac{1}{2}\left(\sqrt{8w + 1} - 1\right)$ is a positive integer.

In fact, it suffices if $8w + 1$ is a square number! Since $8w + 1$ must be an odd number, the square root of $8w + 1$ can only be odd. It follows that $\frac{1}{2}\left(\sqrt{8w + 1} - 1\right)$ can only a positive integer.

With the help of the [`is_square` function](https://docs.sympy.org/latest/modules/ntheory.html#sympy.ntheory.primetest.is_square) from the `sympy` library, the code can then be written in a rather straightforward way. For the more hardcore programmers, you can try to implement the `is_square` method yourself by computing the [integer square root](https://en.wikipedia.org/wiki/Integer_square_root) of the number.

{% highlight python %}
from sympy.ntheory.primetest import is_square
from urllib.request import urlopen


def is_triangle(n):
    return is_square(8 * n + 1)


def word_value(word):
    return sum([char_value(c) for c in word])


def char_value(c):
    return ord(c.upper()) - ord("A") + 1


words = []
data = urlopen("https://projecteuler.net/project/resources/p042_words.txt")
for line in data:
    words += line.decode("utf-8").replace('"', "").split(",")

print(sum([1 for word in words if is_triangle(word_value(word))]))
{% endhighlight %}