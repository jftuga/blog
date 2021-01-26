---
layout: post
title: "Enabling Aruba ClearPass on a switch port"
date: 2021-01-26 20:50:54 +0000
categories: aruba clearpass
tags: aruba clearpass
excerpt: Automating ClearPass port assignment
---


## gist: [Enabling Aruba ClearPass on a switch port](https://gist.github.com/jftuga/94a8b858ef80ec1c9168aa0d1bde440b)

**File:** enable_clearpass.bat

```
@echo off
setlocal

set PORT=%1
if not defined PORT goto ERR

@echo conf t
@echo vlan 1 untag %PORT%
@echo no port-security %PORT%
@echo aaa port-access authenticator %PORT% client-limit 1
@echo aaa port-access authenticator %PORT% tx-period 10
@echo aaa port-access %PORT% auth-order mac-based authenticator
@echo aaa port-access %PORT% auth-priority authenticator mac-based
@echo aaa port-access authenticator %PORT%
@echo aaa port-access mac-based %PORT%
@echo int %PORT% enable
@echo sho mac-address ^| include %PORT%
@echo sho port-access clients ^| include %PORT%
goto END

:ERR
@echo Give PORT on cmd line
goto END

:END
endlocal

```

