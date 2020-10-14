---
layout: post
title: "Which Python linter do you use in VS Code?"
date: 2020-05-11 05:56:08 +0000
categories: python
---

VS Code gives the following choices for a linter:

* bandit
* flake8
* mypy
* prospector
* pycodestyle
* pydocstyle
* pylama
* pylint

Which one do you use and why?

___


* [Thorough Reddit Diecussion](https://www.reddit.com/r/Python/comments/jar4rd/linters_which_one/)
* [Original Reddit Discussion](https://www.reddit.com/r/Python/comments/gheine/which_linter_do_you_use_in_vs_code/)

___

**Update:** October 12, 2020

I want to use `VS Code` integration while linting remotely on a *Raspberry Pi 4*.  Speed is important in this scenario.
This is a very rudimentary benchmark...

Program | Speed
--------| -----
bandit      | **not a linter**
flake8      | **fast**
mypy        | **very slow**
prospector  | **very slow**
pycodestyle | **not a linter**
pydocstyle  | **not a linter**
pylama      | **good**
pylint      | **slow**

___

For `VS Code`, I am using the following `Flake8 Args`.  This is simply to accommodate my own coding style.

* --ignore
* E201,E202,E226,E231,E302,E501

See also: [https://code.visualstudio.com/docs/python/linting](https://code.visualstudio.com/docs/python/linting)

