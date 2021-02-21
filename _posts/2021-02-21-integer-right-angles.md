---
layout: post
title: "Problem 039: Integer Right Triangles"
date: 2021-02-21 12:09:09 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
If $p$ is the perimeter of a right angle triangle with integral length sides, $\\{a,b,c\\}$, there are exactly three solutions for $p = 120$.

$\\{20,48,52\\}$, $\\{24,45,51\\}$, $\\{30,40,50\\}$

For which value of $p \leq 1000$, is the number of solutions maximised?

## Solution
I went for a brute-force solution, finding all tuples of $(a, b, c)$'s such that these criteria are reached:
* $a \leq b \leq c$
* $a + b + c \leq 1000$
* $a^2 + b^2 = c^2$

For each tuple $(a, b, c)$ with the above criteria, I store them to a dictionary $D: \mathbb{N} \to \mathcal{P}\left(\mathbb{N}^3\right)$, where this eventually should hold for all $p \leq 1000$:

\\\[ D(p) = \left\\{ (a, b, c) \mid a + b + c = p \wedge a^2 + b^2 = c^2 \wedge a \leq b \leq c \right\\} \\\]

We then just need to find the $p \leq 1000$ such that $\left\lvert D(p) \right\rvert$ is maximized.

It might seem difficult to find such tuples, but it really isn't. You just need three nested for-loops:
{% highlight python %}
# Maps permieters to valid triangle solutions
D = {}

for a in range(1, 1001):
    for b in range(a, 1001):
        for c in range(b, 1001 - a - b):
            if a * a + b * b == c * c:
                p = a + b + c
                if p in D:
                    D[p].append((a, b, c))
                else:
                    D[p] = [(a, b, c)]
{% endhighlight %}

The ranges can be further optimized. Since $a \leq b \leq c$ and $a + b + c \leq 1000$, it follows that $a \leq \left\lfloor \frac{1000}{3} \right\rfloor = 333$. Similarly, $b$ is bounded above by $\frac{1000 - a}{2}$. Therefore we can rewrite the first two for-loop iterations as:

{% highlight python %}
for a in range(1, 1000 // 3 + 1):
    for b in range(a, (1000 - a) // 2 + 1):
{% endhighlight %}

The optimal $p$ can be found with this piece of code:
{% highlight python %}
max_record = max(D.items(), key=(lambda item: len(item[1])))
print(max_record)
{% endhighlight %}