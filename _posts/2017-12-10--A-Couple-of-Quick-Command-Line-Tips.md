---
layout: post
title: "A Couple of Quick Command Line Tips"
date: 2017-12-10 04:20:35 +0000
categories: sysadmin windows command-line
---

If you just want a quick way to create or append to a text file, you can do something this:

```
    type con >> groceries.txt
    milk
    butter
    eggs
    ^Z
```

where ^ Z is control-Z.

If you want to run a cmd-line program, but not see the output, you can run:

    mypgm.exe > nul

However, this will still show you errors that are sent to STDERR.  Those can also be redirected:

    mypgm.exe > nul 2>&1

2 is the STDERR file descriptor and 1 is the STDOUT file descriptor.  So you can then redirect both of these to a file:

    mypgm.exe > results.txt 2>&1

Or separate them:

    mypgm.exe > results.txt 2> errors.txt

You can also pipe a command to *cmd*.  This is a contrived example, but you get the idea:

    `echo ping 8.8.8.8 | cmd`
