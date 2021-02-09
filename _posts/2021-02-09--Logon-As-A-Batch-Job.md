---
layout: post
title: "Granting Logon as a Batch Job privilege on Windows Server Core"
date: 2021-02-09 18:39:22 +0000
categories: command-line sysadmin windows c#
tags: command-line sysadmin windows c#
excerpt: How to grant 'Logon as a Batch Job' on Windows Server Core
---

The `logon as a batch job` privilege is required when creating a task
from within `Task Scheduler`.  Setting this is normally accomplished
with the `Local Security Policy` *(secpol.msc)* control.  However,
this program is not included in `Windows Server Core`, thus making it
difficult to configure.  This program solves this problem as it is a
command-line tool.  In order for successful operation, it must be used
in the `Run As Administrator` context.

Windows command-line program:
* [logon_as_batch_job](https://github.com/jftuga/logon_as_batch_job)

