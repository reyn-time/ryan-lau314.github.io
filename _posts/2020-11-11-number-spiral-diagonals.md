---
layout: post
title: "Problem 028: Number Spiral Diagonals"
date: 2020-11-11 21:27:27 +0000
tags: euler
---

{% include mathjax.html %}

## Problem

Starting with the number 1 and moving to the right in a clockwise direction a 5 by 5 spiral is formed as follows:

<pre style="text-align:center">
<span style="color:red;font-weight:bold">21</span> 22 23 24 <span style="color:red;font-weight:bold">25</span>
20 <span style="color:red;font-weight:bold"> 7</span>  8 <span style="color:red;font-weight:bold"> 9</span> 10
19  6 <span style="color:red;font-weight:bold"> 1</span>  2 11
18 <span style="color:red;font-weight:bold"> 5</span>  4 <span style="color:red;font-weight:bold"> 3</span> 12
<span style="color:red;font-weight:bold">17</span> 16 15 14 <span style="color:red;font-weight:bold">13</span>
</pre>

It can be verified that the sum of the numbers on the diagonals is 101.

What is the sum of the numbers on the diagonals in a 1001 by 1001 spiral formed in the same way?

## Solution
If we look at the upper right corner of the 5 by 5 square, we can see that the number is $25 = 5^2$.

This is not a coincidence. For all $(2N + 1)$ by $(2N + 1)$ squares, the last number in the spiral - $(2N + 1)^2$ will always land on the upper right corner. By considering the side length of the square, the other three corners will always have the values $(2N + 1)^2 - 2N$, $(2N + 1)^2 - 4N$ and $(2N + 1)^2 - 6N$. Therefore the sum of the four corners is:
\\\[ 4(2N+1)^2 - 12N = 16N^2 + 4N + 4 \\\]

To compute the sum of diagonals in the $(2N + 1)$ by $(2N + 1)$ square, we simply need to find the solution of:
\\\[ 1 + \\sum_{n=1}^N \\left(16n^2 + 4n + 4\\right) \\\]

By applying the following well-known equations:
\\\[ \sum_{n=1}^N 1 = N \\\]
\\\[ \sum_{n=1}^N n = \frac{1}{2}N(N+1) \\\]
\\\[ \sum_{n=1}^N n^2 = \frac{1}{6}N(N+1)(2N+1) \\\]

The solution can be further simplified into this expression:
\\\[ 1 + \frac{8}{3} N(N+1)(2N+1) + 2N(N+1) + 4N \\\]

For a 1001 by 1001 spiral, $N=500$. Therefore the sum of the diagonal is:
\\\[ 1 + \frac{8}{3} \cdot 500 \cdot 501 \cdot 1001 + 2 \cdot 500 \cdot 501 + 4 \cdot 500 = 669171001 \\\]

