---
layout: post
title: "Problem 001: Multiples of 3 and 5"
date: 2020-07-18 15:35:14 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.

## Solution
Here's a straightforward way of summing up those multiples:

{% highlight python %}
sum = 0
for i in range(1000):
    if i % 3 == 0 or i % 5 == 0:
        sum += i
print(sum)
{% endhighlight %}

Note that it is not hard to calculate this by hand too:

$$\begin{align}
\text{Sum of multiples of 3 and 5} &= \text{Sum of multiples of 3} + \text{Sum of multiples of 5} - \text{Sum of multiples of 15}\\
&= (3 + 6 + \cdots + 999) + (5 + 10 + \cdots + 995) - (15 + 30 + \cdots + 990)\\
&= 3 \times (1 + \cdots + 333) + 5 \times (1 + \cdots + 199) - 15 \times (1 + \cdots + 66)\\
&= 3 \times \frac{(1 + 333) \times 333}{2} + 5 \times \frac{(1 + 199) \times 199}{2} - 15 \times \frac{(1 + 66) \times 66}{2}\\
&= 166833 + 99500 - 33165\\
&= 233168
\end{align}$$

If we are trying to find the sum of multiples of 3 and 5 below a larger value, say 1,000,000,000, the latter method would be much more time efficient.