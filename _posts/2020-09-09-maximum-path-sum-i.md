---
layout: post
title: "Problem 018: Maximum Path Sum I"
date: 2020-09-10 20:00:48 +0100
tags: euler
---
{% include mathjax.html %}
## Problem 
By starting at the top of the triangle below and moving to adjacent numbers on the row below, the maximum total from top to bottom is 23.
<pre style="text-align:center">
<span style="color:red;font-weight:bold">3</span>
<span style="color:red;font-weight:bold">7</span> 4
2 <span style="color:red;font-weight:bold">4</span> 6
8 5 <span style="color:red;font-weight:bold">9</span> 3
</pre>
That is, $3 + 7 + 4 + 9 = 23$.

Find the maximum total from top to bottom of the triangle below:
<pre style="text-align:center">
75
95 64
17 47 82
18 35 87 10
20 04 82 47 65
19 01 23 75 03 34
88 02 77 73 07 63 67
99 65 04 28 06 16 70 92
41 41 26 56 83 40 80 70 33
41 48 72 33 47 32 37 16 94 29
53 71 44 65 25 43 91 52 97 51 14
70 11 33 28 77 73 17 78 39 68 17 57
91 71 52 38 17 14 91 43 58 50 27 29 48
63 66 04 68 89 53 67 30 73 16 69 87 40 31
04 62 98 27 23 09 70 98 73 93 38 53 60 04 23
</pre>

NOTE: As there are only 16384 routes, it is possible to solve this problem by trying every route. However, [Problem 67](https://projecteuler.net/problem=67), is the same challenge with a triangle containing one-hundred rows; it cannot be solved by brute force, and requires a clever method! ;o)
## Solution
This problem shares some similarities with [Problem 015: Lattice Paths]({% post_url 2020-08-19-lattice-paths %}).

Suppose the whole triangle is stored in a 2D array $A$, where $A[i][j]$ is the value of the cell in the $i$-th row, $j$-th column (say $(i, j)$).

Let $f(i, j)$ be the maximum path sum from $(0, 0)$ to $(i, j)$. For the 15-row triangle above, we would like to find the following value:
\\\[ \max_{0\leq i<15} f(14, i) \\\]

In general, if a path passes through the cell $(i, j)$, the path must have previously walked past either $(i-1, j-1)$ or $(i-1, j)$. Therefore, this general recurrence relation must hold for most $i, j$'s:
\\\[ f(i, j) = A[i][j] + max\big(f(i-1, j-1), f(i-1, j)\big) \\\]

We also have to take care of edge cases (e.g. cells on the leftmost/rightmost column), where the route can only come from one possible cell. The following Python code can be used as a reference.
{% highlight python %}
for i in range(len(A)):
    for j in range(i + 1):
        if i == 0:
            f[i][j] = A[i][j]
        elif j == 0:
            f[i][j] = A[i][j] + f[i - 1][j]
        elif j == i:
            f[i][j] = A[i][j] + f[i - 1][j - 1]
        else:
            f[i][j] = A[i][j] + max(f[i - 1][j], f[i - 1][j - 1])

  print(max([f[len(A) - 1][j] for j in range(len(A))]))
{% endhighlight %}