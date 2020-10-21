---
layout: post
title: "Problem 025: 1000-digit Fibonacci Number"
date: 2020-10-21 10:02:34 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The Fibonacci sequence is defined by the recurrence relation:

$F_n = F_{n−1} + F_{n−2}$, where $F_1 = 1$ and $F_2 = 1$.

Hence the first 12 terms will be:
* $F_1 = 1$
* $F_2 = 1$
* $F_3 = 2$
* $F_4 = 3$
* $F_5 = 5$
* $F_6 = 8$
* $F_7 = 13$
* $F_8 = 21$
* $F_9 = 34$
* $F_{10} = 55$
* $F_{11} = 89$
* $F_{12} = 144$

The 12th term, $F_{12}$, is the first term to contain three digits.

What is the index of the first term in the Fibonacci sequence to contain 1000 digits?
## Solution
Given that you can generate Fibonacci numbers, and that your program can handle big numbers, it is not hard to solve the problem.

I am using the Fibonacci generator created [many posts ago]({% post_url 2020-07-22-even-fibonacci-numbers %}).

I compute the number of digits in a number $n$ by checking the length of $n$ in string form, i.e. `len(str(n))`.

{% highlight python %}
i = 0
generator = fib()
curr_fib = next(generator)

while len(str(curr_fib)) < 1000:
    curr_fib = next(generator)
    i += 1

print(f"{i}: {curr_fib}")
{% endhighlight %}
