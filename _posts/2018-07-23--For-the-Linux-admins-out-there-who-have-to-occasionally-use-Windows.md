---
layout: post
title: "For the Linux admins out there who have to occasionally use Windows..."
date: 2018-07-23 06:04:52 +0000
categories: command-line sysadmin
tags: command-line sysadmin
excerpt: A short similarity list between the Windows and Linux command lines
---

For the Linux admins out there who have to occasionally use Windows, you can do the following from a Windows cmd prompt:

* *pushd* and *popd*
* use *nul* for /dev/null
* use *con* for stdin, example: *type con > myfile.txt*
* you can pipe commands to cmd (similar to piping to sh): `whatever | cmd`
* Unix command line utilities for Windows: https://github.com/bmatzelle/gow
* these are the same: ping, arp, netstat, route
* traceroute is *tracert*
* grep is *findstr* with slightly different parameters.
* *robocopy* is (more-or-less) the Windows version of rsync
* Use double quotes around a cmd-line argument that has spaces to pass it in to the program as a single parameter
