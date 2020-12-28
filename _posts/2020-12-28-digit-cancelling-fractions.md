---
layout: post
title: "Problem 033: Digit Cancelling Fractions"
date: 2020-12-28 15:52:43 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The fraction $\frac{49}{98}$ is a curious fraction, as an inexperienced mathematician in attempting to simplify it may incorrectly believe that $\frac{49}{98} = \frac{4}{8}$, which is correct, is obtained by cancelling the 9s.

We shall consider fractions like, $\frac{30}{50} = \frac{3}{5}$, to be trivial examples.

There are exactly four non-trivial examples of this type of fraction, less than one in value, and containing two digits in the numerator and denominator.

If the product of these four fractions is given in its lowest common terms, find the value of the denominator.

## Solution
A question that could be solved with brute force. All we have to do is to find all $a, b, c \in [0, 9]$ such that
\\\[ 
\begin{equation} 
\begin{split} 
\frac{c \mathbin\Vert a}{c \mathbin\Vert b} &= \frac{a}{b} \\\\\\
\frac{c \mathbin\Vert a}{b \mathbin\Vert c} &= \frac{a}{b} \\\\\\
\frac{a \mathbin\Vert c}{c \mathbin\Vert b} &= \frac{a}{b} \\\\\\
\frac{a \mathbin\Vert c}{b \mathbin\Vert c} &= \frac{a}{b}
\end{split}
\end{equation}\\\]

Care must be taken to avoid unwanted or trivial fractions (say $\frac{A}{B}$) on the left side. These criteria have to be satisifed:
* $B > A \geq 10$.
* At least one of $A$ and $B$ is not a multiple of 10.

Here's how I wrote the brute-force code in Python. I used the `fractions` library to make my life slightly easier.
{% highlight python %}
from fractions import Fraction

def is_curious_solution(A, B, a, b):
    return 1 if B >= 10 and A < B and not (A % 10 == 0 and B % 10 == 0) and Fraction(A, B) == Fraction(a, b) else 0

def curious_solution_count(a, b, c):
    # (ca/cb,ca/bc,ac/cb,ac/bc) = a/b
    return is_curious_solution(c * 10 + a, c * 10 + b, a, b) \
        + is_curious_solution(c * 10 + a, b * 10 + c, a, b) \
            + is_curious_solution(a * 10 + c, c * 10 + b, a, b) \
                + is_curious_solution(a * 10 + c, b * 10 + c, a, b) 

curious_solutions = []

for a in range(1, 10):
    for b in range(a + 1, 10):
        for c in range(10):
            n = curious_solution_count(a, b, c)
            for i in range(n):
                curious_solutions.append(Fraction(a, b))

product = Fraction(1, 1) 
for solution in curious_solutions:
    product *= solution

print(product.denominator)
{% endhighlight %}