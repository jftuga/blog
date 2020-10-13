---
layout: post
title: "How to create a Scheduled Task with PowerShell"
date: 2020-07-17 11:43:37 +0000
categories: powershell
---


## [How to create a Scheduled Task with PowerShell](https://gist.github.com/jftuga/da9558855bf3a077a8cb7077ad4bcf7c)

**File:** create_scheduled_task.ps1

```
# This task will run once per day at 9AM, only on week days

$A = New-ScheduledTaskAction -Execute "c:\scripts\my_project\scheduled_task.bat" -WorkingDirectory "c:\scripts\my_project"
$T = New-ScheduledTaskTrigger -At 9am -Weekly -DaysofWeek Monday,Tuesday,Wednesday,Thursday,Friday
$P = New-ScheduledTaskPrincipal -UserId "LOCALSERVICE" -LogonType ServiceAccount
$S = New-ScheduledTaskSettingsSet
$D = New-ScheduledTask -Action $A -Principal $P -Trigger $T -Settings $S
Register-ScheduledTask "My Project" -InputObject $D

```

