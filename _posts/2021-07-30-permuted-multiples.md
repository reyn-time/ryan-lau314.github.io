---
layout: post
title: "Problem 052: Permuted Multiples"
date: 2021-07-30 20:05:48 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
It can be seen that the number, 125874, and its double, 251748, contain exactly the same digits, but in a different order.

Find the smallest positive integer, $x$, such that $2x$, $3x$, $4x$, $5x$, and $6x$, contain the same digits.

## Solution
Suppose there are $n$ digits in $x$. If $x$ and $6x$ contain the same number of digits, the following two inequalities must hold:

\\\[  
\begin{equation}
    \begin{cases}
      10^{n-1} \leq x < 10^n \\\\\
      10^{n-1} \leq 6x < 10^n \\\\\
    \end{cases}    
\end{equation}   
\\\]

When simplified, we get the following:
\\\[ 10^{n-1} \leq x < \frac{10^n}{6} \\\]

This gives us a smaller search space for $x$, given the number of digits $n$.

It is not hard to test if $kn$ is a permutated multiple of $n$. In the code below I simply sorted the digits in both numbers, and see if they match. Not terribly efficient, but it works.

{% highlight python %}
def is_permutated_multiple_of(n, factor):
    return sorted(str(n)) == sorted(str(n * factor))
{% endhighlight %}

Using this function it is not hard to verify if $n$ is a solution for the problem:

{% highlight python %}
def is_solution(n):
    for factor in range(2, 7):
        if not is_permutated_multiple_of(n, factor):
            return False
    return True
{% endhighlight %}

Then we just need a clever way of exhaustion. Try all numbers with 1 digit, then try all with 2 digits and so on... eventually we can find the minimal number with such property:

{% highlight python %}
def p52():
    for num_of_digits in count(1):
        start = 10 ** (num_of_digits - 1)
        end = 10 ** num_of_digits // 6
        for n in range(start, end + 1):
            if is_solution(n):
                print(n)
                return
{% endhighlight %}

The solution turns out to be 142857. Note that the decimal expansion of $1/7$ has the same digits, i.e. $0.\overline{142857}$. Is this a coincidence?