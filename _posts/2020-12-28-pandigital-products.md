---
layout: post
title: "Problem 032: Pandigital Products"
date: 2020-12-28 14:42:27 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
We shall say that an $n$-digit number is pandigital if it makes use of all the digits 1 to $n$ exactly once; for example, the 5-digit number, 15234, is 1 through 5 pandigital.

The product 7254 is unusual, as the identity, $39 \times 186 = 7254$, containing multiplicand, multiplier, and product is 1 through 9 pandigital.

Find the sum of all products whose multiplicand/multiplier/product identity can be written as a 1 through 9 pandigital.

HINT: Some products can be obtained in more than one way so be sure to only include it once in your sum.

## Solution
It is incredibly useful to know that pandigital products must have exactly 4 digits.

Let the multiplicand, multiplier and product be $a$, $b$ and $ab$ respectively. Let $len: \mathbb{N}\to\mathbb{N}$ be the function that computes the number of digits for a number.

It can be proven that:
\\\[\begin{equation}len(a) + len(b) - 1 \leq len(ab) \leq len(a) + len(b) \tag{1}\label{a} \end{equation} \\\]
 (It is a bit tedious, so I will attach the proof later.)

For $ab$ to be a pandigital product, all digits from 1 to 9 must be used exactly once in $a$, $b$, $ab$ combined. Hence:
\\\[len(a) + len(b) + len(ab) = 9 \\\]

It can be seen that $len(ab) \not= len(a) + len(b)$. If it holds, the equation would be even on the left and odd on the right. Therefore by inequality $\ref{a}$, $len(ab) = len(a) + len(b) - 1$. It follows that $len(ab) = 4$. 

The follow psuedocode can then be used to find all pandigital products:
* For all $n \in [1000, 9999]$:
  * For all $a \in [1, \sqrt{n})$:
    * If $n$ is divisible by $a$ and $(a, n / a, n)$ forms a pandigital product:
        * Print $n$

It is not very hard to code in Python:
{% highlight python %}
def uses_one_to_nine_exactly_once(*values):
    char_list = [char for value in values for char in str(value)]
    char_list.sort()
    return ''.join(char_list) == '123456789'

def is_pandigital(n):
    i = 1
    while i * i <= n:
        if n % i == 0:
            if uses_one_to_nine_exactly_once(n, i, n // i):
                print(f'{i} * {n // i} = {n}')
                return True
        i += 1
    return False

print([n for n in range(1000, 10000) if is_pandigital(n)])
{% endhighlight %}


### Proof of inequality
Firstly, notice that the following statement holds for all $n, N \in \mathbb{N}$:
\\\[len(n) = N \iff 10^{N-1} \leq n < 10^N \tag{2}\label{b} \\\]

Consider arbitrary $a, b \in \mathbb{N}$. 

For simplicity let $len(a) = A$ and $len(b) = B$ for some $A, B$.

By $\ref{b}$, $10^{A-1} \leq a < 10^A$ and $10^{B-1} \leq b < 10^B$. Therefore:
\\\[ 10^{A+B-2} \leq ab < 10^{A+B} \\\]

Applying $\ref{b}$ again, we can show that $len(ab) = A+B$ or $A+B-1$. We are done.