---
layout: post
title: "Problem 024: Lexicographic Permutations"
date: 2020-10-20 21:12:48 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
A permutation is an ordered arrangement of objects. For example, 3124 is one possible permutation of the digits 1, 2, 3 and 4. If all of the permutations are listed numerically or alphabetically, we call it lexicographic order. The lexicographic permutations of 0, 1 and 2 are:

<p style="text-align:center" >012 021 102 120 201 210</p> 

What is the millionth lexicographic permutation of the digits 0, 1, 2, 3, 4, 5, 6, 7, 8 and 9?
## Solution
There is a slightly-hard-to-explain algorithm on finding the next permutation of a list. Let's say we are going to find the next permutation of the list $A$:
<pre>3 1 4 5 9 8 2 0</pre>
First, find the *rightmost* index $i$ such that $A[i] < A[i+1]$. 
<pre>
3 1 4 <span style="color:red;font-weight:bold">5</span> 9 8 2 0
      i
</pre>
This essentially means that all array values after index $i$ are in descending order.

We can prove all array values before index $i$ will not be changed in the next permutation (?).

Next, we find the *rightmost* index $j$ such that $A[j] > A[i]$:
<pre>
3 1 4 <span style="color:red;font-weight:bold">5</span> 9 <span style="color:green;font-weight:bold">8</span> 2 0
      i   j
</pre>  
Swap the array values indexed at $i$ and $j$:
<pre>
3 1 4 <span style="color:green;font-weight:bold">8</span> 9 <span style="color:red;font-weight:bold">5</span> 2 0
      i   j
</pre>
Reverse the sublist from $i+1$ to the end:
<pre>
3 1 4 <span style="color:green;font-weight:bold">8</span> 0 2 5 9
      i <----->
</pre>
...and that's the next permutation.

Now we just need to write the algorithm. Here's how I've written it:
{% highlight python %}
def next_permutation(array):
    i = __find_rightmost_rise(array)

    if i is None:
        return False

    j = __find_next_largest_number_index_after_ith(array, i)
    array[i], array[j] = array[j], array[i]
    array[i + 1 :] = array[i + 1 :][::-1]
    return True


def __find_rightmost_rise(array):
    """
    Finds the largest i such that array[i] < array[i + 1].
    If there is no such i, return None.
    """
    if len(array) == 1:
        return None

    i = len(array) - 2
    while i >= 0:
        if array[i] < array[i + 1]:
            return i
        i -= 1

    return None


def __find_next_largest_number_index_after_ith(array, i):
    """
    Finds the largest j such that array[j] > array[i].
    """
    for j in range(len(array) - 1, 0, -1):
        if array[j] > array[i]:
            return j
{% endhighlight %}

Using this algorithm, it is not hard to find the millionth permutation of [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]:
{% highlight python %}
array = [str(i) for i in range(10)]
for i in range(1_000_000 - 1):
    next_permutation(array)
    
print("".join(array))
{% endhighlight %}