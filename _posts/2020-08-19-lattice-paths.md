---
layout: post
title: "Problem 015: Lattice Paths"
date: 2020-08-19 09:53:42 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
Starting in the top left corner of a $2 \times 2$ grid, and only being able to move to the right and down, there are exactly 6 routes to the bottom right corner.

<img style="display:block;margin:auto" src="https://projecteuler.net/project/images/p015.png" />

How many such routes are there through a $20 \times 20$ grid?

## Solution
Let's define things mathematically before tackling the problem.

Let $(0, 0), (20, 20)$ be the top left and bottom right corners of the $20 \times 20$ grid respectively. 

For every integer coordinates $(x, y)$, let $f(x, y)$ be the number of ways to reach the point. 

We aim to find $f(20, 20)$.

Two important observations:
* When $x = 0$ or $y = 0$:
    * $f(x, y) = 1$. (You can only walk straight down / right.)
* When $x > 0$ and $y > 0$:
    * Consider points $P_1 = (x - 1, y)$ and $P_2 = (x, y - 1)$. If a route reaches $(x, y)$, it must have passed either $P_1$ or $P_2$.
    * It follows that $f(x, y) = f(x - 1, y) + f(x, y - 1)$. (Why?)

Hence, we get this recurrence relation:
\\\[
\begin{equation}
  f(x, y) =
    \begin{cases}
      1 &(\text{when } x = 0 \text{ or } y = 0) \\\\\\
      f(x - 1, y) + f(x, y - 1) &(\text{otherwise}) \\\\\\
    \end{cases}       
\end{equation}    
\\\]

By evaluating and [memoizing](https://en.wikipedia.org/wiki/Memoization) all $f(x, y)$'s in a specific order, it is not hard to compute $f(20, 20)$:
{% highlight python %}
dp = [[0 for _ in range(21)] for _ in range(21)]

for i in range(21):
    for j in range(21):
        if i == 0 or j == 0:
            dp[i][j] = 1
        else:
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
            
print(dp[20][20])
{% endhighlight %}