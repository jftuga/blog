---
layout: post
title: "Use git --follow to review commit history of a single file"
date: 2020-10-25 13:00:02 +0000
categories: git programming
tags: git programming
excerpt: How to see the commits of just one file with in a git repo
---

It is possible to review *all* of the commits of a single file instead of all files within a single commit, multiple commits, etc.

This is accomplished by using: `git log --follow`

```
--follow
Continue listing the history of a file beyond renames (works only for a single file).
```

Example:

{% highlight shell %}
git clone https://github.com/git/git.git
git log --follow -- git/builtin/commit.c
{% endhighlight %}

For each change to this file, the output will include:

* The commit hash
* Author
* Date
* Commit message
