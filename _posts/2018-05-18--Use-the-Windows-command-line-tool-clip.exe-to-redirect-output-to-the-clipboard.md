---
layout: post
title: "Use the Windows command-line tool, clip.exe, to redirect output to the clipboard"
date: 2018-05-18 21:34:43 +0000
categories: command-line
tags: command-line
excerpt: A short tutorial on how to paste into the system clipboard from the Windows command-line
---

Windows 10 (not sure about earlier versions) has a built-in program, *clip.exe*, that lets you redirect command-line output to the clipboard.

Examples:

{% highlight batch linenos %}
dir | clip
type foo.txt | findstr /i "bar" | clip
{% endhighlight %}

___

[Reddit discussion](https://www.reddit.com/r/commandline/comments/8kez7k/use_the_windows_commandline_tool_clipexe_to/)
