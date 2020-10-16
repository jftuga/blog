---
layout: post
title: "Which Python linter do you use in VS Code?"
date: 2020-05-11 05:56:08 +0000
categories: 2020
tags: python
excerpt: A summary of the research I did into Python linting.
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

* [Original Reddit Discussion](https://www.reddit.com/r/Python/comments/gheine/which_linter_do_you_use_in_vs_code/)
* [Thorough Reddit Diecussion](https://www.reddit.com/r/Python/comments/jar4rd/linters_which_one/)

___

**Update:** October 12, 2020

Since I want to use `VS Code` integration while linting remotely on a *Raspberry Pi 4*, speed is important in this scenario.
This is my *very rudimentary* benchmark of several linters.

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

___

**See Also**

* [Linting Python in Visual Studio Code](https://code.visualstudio.com/docs/python/linting)
* [Curated awesome list of flake8 extensions](https://github.com/DmytroLitvinov/awesome-flake8-extensions)

