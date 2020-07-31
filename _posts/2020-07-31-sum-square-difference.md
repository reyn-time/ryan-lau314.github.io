---
layout: post
title: "Problem 006: Sum Square Difference"
date: 2020-07-31 09:35:10 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
The sum of the squares of the first ten natural numbers is,
\\\[1^2 + 2^2 + \cdots + 10^2 = 385\\\]

The square of the sum of the first ten natural numbers is,
\\\[(1 + 2 + \cdots + 10)^2 = 3025\\\]

Hence the difference between the sum of the squares of the first ten natural numbers and the square of the sum is $3025-385=2640$.

Find the difference between the sum of the squares of the first one hundred natural numbers and the square of the sum.

## Solution
I suppose many still remember the formula to the sum of arithmatic sequence $1, 2, \cdots, n$:
\\\[1 + 2 + \cdots + n = \frac{1}{2}n(n+1)\\\]

Surprisingly, there is also a simple formula for computing the sum of squares:
\\\[1^2 + 2^2 + \cdots + n^2 = \frac{1}{6}n(n+1)(2n+1)\\\]

By the two formula above, it is not hard to compute the solution manually.

## Notes
You might be thinking, how do people come up with that sum of squares equation? It is not that hard!

Consider the following expression:
\\\[\sum_{i=1}^N(i+1)^3 - \sum_{i=1}^Ni^3\\\]

Using the properties of summation, one should be able to simplify it to this form:
\\\[
\begin{equation}
\begin{split} 
\sum_{i=1}^N(i+1)^3 - \sum_{i=1}^Ni^3 &= \sum_{i=1}^N\big((i+1)^3-i^3\big) \\\\\\
&= \sum_{i=1}^N(i^3 + 3i^2 + 3i + 1 - i^3) \\\\\\
&= 3\sum_{i=1}^Ni^2 + 3\sum_{i=1}^Ni
\end{split} 
\end{equation}
\\\]

By observing the expanded form, we can also derive another simplified expression:
\\\[
\begin{equation}
\begin{split} 
\sum_{i=1}^N(i+1)^3 - \sum_{i=1}^Ni^3 &= \big(2^3 + 3^3 + \cdots + (N+1)^3\big) - (1^3 + 2^3 + \cdots + N^3) \\\\\\
&= (N+1)^3 - 1
\end{split} 
\end{equation}
\\\]

Therefore, comparing both simplified expressions:
\\\[
\begin{equation}
\begin{split} 
3\sum_{i=1}^Ni^2 + 3\sum_{i=1}^Ni &= (N+1)^3 - 1 \\\\\\
3\sum_{i=1}^Ni^2 + \frac{3}{2}N(N+1) &= (N+1)^3 - 1 \\\\\\
3\sum_{i=1}^Ni^2 &= (N+1)^3 - 1 - \frac{3}{2}N(N+1) \\\\\\
\sum_{i=1}^Ni^2 &= \frac{1}{3}\big((N+1)^3 - 1 - \frac{3}{2}N(N+1)\big) \\\\\\
&= \frac{1}{3}(N^3 + 3N^2 + 3N + 1 - 1 - \frac{3}{2}N^2 - \frac{3}{2}N) \\\\\\
&= \frac{1}{3}(N^3 + \frac{3}{2}N^2 + \frac{3}{2}N) \\\\\\
&= \frac{1}{6}N(2N^2 + 3N + 3) \\\\\\
&= \frac{1}{6}N(N+1)(2N+1)
\end{split} 
\end{equation}
\\\]

Using similar methods, we can derive equations for the sum of cubes too!