---
layout: post
title: "Problem 038: Pandigital Multiples"
date: 2021-02-21 11:57:24 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
Take the number 192 and multiply it by each of 1, 2, and 3:

* $192 \times 1 = 192$
* $192 \times 2 = 384$
* $192 \times 3 = 576$

By concatenating each product we get the 1 to 9 pandigital, 192384576. We will call 192384576 the concatenated product of 192 and (1,2,3).

The same can be achieved by starting with 9 and multiplying by 1, 2, 3, 4, and 5, giving the pandigital, 918273645, which is the concatenated product of 9 and (1,2,3,4,5).

What is the largest 1 to 9 pandigital 9-digit number that can be formed as the concatenated product of an integer with $(1,2, \cdots , n)$ where $n > 1$?

## Solution
Since we need at least two numbers to concatanate, the "seed" integer would have a maximum length of $\left\lfloor \frac{9}{2} \right\rfloor = 4$. We can try all possible "seed" integers from 1 to 9999, and see which seed generates the largest pandigital number. 

Note that not all seeds could generate a 9-digit pandigital number, so we need to check carefully too.

Here is the code I used for generating the pandigital for each seed. 
If a pandigital cannot be generated, the function returns `None`.

{% highlight python %}
def gen_pandigital_for_seed(seed):
    pandigital = ""
    i = 1
    while len(pandigital) < 9:
        pandigital = pandigital + str(seed * i)
        i += 1
    return pandigital if is_pandigital(pandigital) else None


def is_pandigital(str):
    return "".join(sorted(str)) == "123456789"
{% endhighlight %}

Then we just need to iterate through the possible seeds. This part is rather trivial:
{% highlight python %}
best_pandigital = 123456789

for seed in range(1, 10000):
    pandigital = gen_pandigital_for_seed(seed)
    if pandigital and int(pandigital) > best_pandigital:
        best_pandigital = int(pandigital)
        print(f"Record update: {seed} generates {pandigital}!")
{% endhighlight %}