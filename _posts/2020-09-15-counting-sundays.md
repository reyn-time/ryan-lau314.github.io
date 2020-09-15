---
layout: post
title: "Problem 019: Counting Sundays"
date: 2020-09-15 20:14:58 +0100
tags: euler
---
{% include mathjax.html %}
## Problem
You are given the following information, but you may prefer to do some research for yourself.
* 1 Jan 1900 was a Monday.
* Thirty days has September,

  April, June and November.

  All the rest have thirty-one,

  Saving February alone,

  Which has twenty-eight, rain or shine.

  And on leap years, twenty-nine.
* A leap year occurs on any year evenly divisible by 4, but not on a century unless it is divisible by 400.
How many Sundays fell on the first of the month during the twentieth century (1 Jan 1901 to 31 Dec 2000)?

## Solution
One can simply iterate through the months and check what day of the week it is for the first of each month in the 20-th century. 

The only thing that is hard here is to determine if a year is a leap year or not. Here's how I've done it in Python:
{% highlight python %}
def is_leap_year(year):
    return year % 4 == 0 and (year % 100 != 0 or year % 400 == 0)
{% endhighlight %}
It might take some time to understand, but essentially it is what the third bullet point is about.

Attaching the rest of the code here as a reference:
{% highlight python %}
month = 1
year = 1900
day_of_week = 1
sunday_counter = 0

while year <= 2000:
    if day_of_week == 0 and year >= 1901:
        sunday_counter += 1

    if month in [1, 3, 5, 7, 8, 10, 12]:
        day_of_week = (day_of_week + 31) % 7
    elif month in [4, 6, 9, 11]:
        day_of_week = (day_of_week + 30) % 7
    elif is_leap_year(year):
        day_of_week = (day_of_week + 29) % 7

    if month == 12:
        month = 1
        year += 1
    else:
        month += 1
        
print(sunday_counter)
{% endhighlight %}