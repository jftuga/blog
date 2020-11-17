---
layout: post
title: "73 Examples to Help You Master Python's f-strings"
date: 2020-11-07 13:20:59 +0000
categories: python
---

Here is an interesting article (and thread) on [Python f-strings](https://www.reddit.com/r/Python/comments/jpojgg/73_examples_to_help_you_master_pythons_fstrings/).

I recently needed to center a string between two `|` symbols.  The `^` symbol is what tells Python to center the string.

Example:

{% highlight python %}
# 10 is the maximum string length that 'team' will ever be
print(f"| {team:^10} | ")
{% endhighlight %}
