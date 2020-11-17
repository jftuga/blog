---
layout: post
title: "73 Examples to Help You Master Python's f-strings"
date: 2020-11-07 13:20:59 +0000
categories: python
---

Here is an interesting article (and thread) on `Python f-strings`.

In recently needed to center a string between two `|` symbols.  This is how I did it:

{% highlight python %}
# 10 is the maximum string length that 'team' will ever be
print(f"| {team:^10} | ")
{% endhighlight %}
