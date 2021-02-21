---
layout: post
title: "Problem 040: Champernowne's Constant"
date: 2021-02-21 12:35:21 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
An irrational decimal fraction is created by concatenating the positive integers:

<pre style="text-align:center">
0.12345678910<span style="color:red;font-weight:bold">1</span>112131415161718192021...
</pre>

It can be seen that the 12<sup>th</sup> digit of the fractional part is 1.

If $d_n$ represents the n<sup>th</sup> digit of the fractional part, find the value of the following expression.

\\\[d_1 \times d_{10} \times d_{100} \times d_{1000} \times d_{10000} \times d_{100000} \times d_{1000000}\\\]

## Solution
I created a generator for producing the digits of the Champernowne's constant:
{% highlight python %}
def champernowne_generator():
    # Generates the decimal part of the Champernowne's constant
    current = 1
    remainder = str(current)
    while True:
        if len(remainder) > 0:
            c = remainder[0]
            remainder = remainder[1:]
            yield c
        else:
            current += 1
            remainder = str(current)
{% endhighlight %}
The code is rather straightforward. In each iteration I consume one digit of the `remainder` string. Once the `remainder` is exhausted, I set `remainder` to the following integer. 

Making use of the generator, it is not hard to find the required digits and multiply them together:
{% highlight python %}
gen = champernowne_generator()

product = 1
for i in range(1, 1_000_001):
    c = next(gen)
    if i in [1, 10, 100, 1_000, 10_000, 100_000, 1_000_000]:
        print(f"Digit {i} has value {c}")
        product *= int(c)
        
print(product)
{% endhighlight %}