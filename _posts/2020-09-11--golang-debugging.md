---
layout: post
title:  "GoLang Debugging"
date:   2020-09-11 15:14:43 +0000
categories: GoLang dlv Programming
---

## How to start and run `dlv` -- the GoLang debugger

```sh
dlv debug cmd.go -- -f input_file.csv
```

Once inside the debugger:

Command | Explanation
--------|------------
b C:/GitHub.com/jftuga/whatever/cmd.go:52 | break at line 52 *(full path to file should be given to avoid filename comflicts)*
p *something* | to print the value of the *something* variable
n | to execute the next line of code
c | continue

[Official Delve Documentation](https://github.com/go-delve/delve/blob/master/Documentation/usage/dlv.md)
