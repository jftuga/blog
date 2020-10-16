---
layout: post
title: "Is there a program to help you document Python functions?"
date: 2020-09-17 22:08:13 +0000
categories: 2020
tags: python
excerpt: If you use VS Code for Python, this plug-in will improve your development process.
---

Here is an interesting [reddit post](https://www.reddit.com/r/Python/comments/iuoxt0/is_there_a_program_to_help_you_document_python/) that I made on /r/Python

___

I just finished a Python project and now want to go back and document each function while it is still fresh in my head.  Is there a program that will scan your code for functions that don't have a doc-string and then interactively ask you to:

* describe the function
* document each parameter
* document the return value

It would then create a doc-string for you that you could simply copy/paste into your code.

___

The solution that I ended up going with was this `Visual Studio Code` extension: [Python Docstring Generator](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring). This is exactly what I was looking for and it works very well!

