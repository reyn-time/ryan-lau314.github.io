---
layout: post
title: "Problem 022: Names Scores"
date: 2020-10-19 21:19:32 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
Using [names.txt](https://projecteuler.net/project/resources/p022_names.txt) (right click and 'Save Link/Target As...'), a 46K text file containing over five-thousand first names, begin by sorting it into alphabetical order. Then working out the alphabetical value for each name, multiply this value by its alphabetical position in the list to obtain a name score.

For example, when the list is sorted into alphabetical order, COLIN, which is worth $3 + 15 + 12 + 9 + 14 = 53$, is the 938th name in the list. So, COLIN would obtain a score of $938 \times 53 = 49714$.

What is the total of all the name scores in the file?
## Solution
A string manipulation / file reading problem. Not very theoretical (sadly).

The `ord()` function finds the Unicode value of a character. Since `A`, `B`, ..., `Z` have consecutive Unicode values, the word score can be easily computed with a smart application of the `ord()` function:
{% highlight python %}
def char_score(c):
    return ord(c.upper()) - ord("A") + 1


def word_score(word, position):
    return sum([char_score(c) for c in word]) * (position + 1)
{% endhighlight %}

The only hard part remaining is to read and parse the file from the URL. I find `urlopen()`, `replace()` and `split()` quite useful here:
{% highlight python %}
from urllib.request import urlopen

names = []
data = urlopen("https://projecteuler.net/project/resources/p022_names.txt")
for line in data:
    names += line.decode("utf-8").replace('"', "").split(",")

names.sort()
print(sum([word_score(name, i) for i, name in enumerate(names)]))
{% endhighlight %}