---
layout: post
title:  "In case you didn't know: Python 3.8 f-strings support = for self-documenting expressions and debugging"
date:   2020-08-26 01:59:10 +0000
categories: programming python
tags: programming python
excerpt: Using the = sign within f-strings can speed up Python 3.8 debugging.
---

Python 3.8 added an `=` specifier to `f-strings`. An `f-string` such as `f'{expr=}'` will expand to the text of the expression, an equal sign, then the representation of the evaluated expression. 

**Examples:**

____

input:

{% highlight python linenos %}
from datetime import date
user = 'eric_idle'
member_since = date(1975, 7, 31)
f'{user=} {member_since=}'
{% endhighlight %}

output:

    "user='eric_idle' member_since=datetime.date(1975, 7, 31)"

____

input:

{% highlight python linenos %}
delta = date.today() - member_since
f'{user=!s}  {delta.days=:,d}'
{% endhighlight %}

output *(no quotes; commas)*:

    'user=eric_idle  delta.days=16,075'

____

input:

{% highlight python linenos %}
from math import cos,radians
theta=30
print(f'{theta=}  {cos(radians(theta))=:.3f}')
{% endhighlight %}

output:

    theta=30  cos(radians(theta))=0.866

____

[Reddit discussion](https://www.reddit.com/r/Python/comments/igq1pg/in_case_you_didnt_know_python_38_fstrings_support/)
