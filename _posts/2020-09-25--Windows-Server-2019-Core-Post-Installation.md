---
layout: post
title: "Windows Server 2019 Core Post Installation"
date: 2020-09-25 13:34:00 +0000
categories: windows command-line
---

Here is a quick *TO DO* list of things that need to be done after a OOB Server 2019 Core install.  These steps should also be executed for the *Desktop Experience* version, as well. I've had this list for a while now, but thought it would be good to post it.

## Basic Setup
* `sconfig.exe`
* Date / Time, Time Zone
* Install (or Update) VMWare Tools, mount ISO and then run d:\setup64.exe
* change IP in network settings
* change telemetry to Security
* change domain, which will also ask you to change computer name (On Desktop version, run sysdm.cpl)
* Windows Activation should be performed by a KMS server.
* download / install **ALL** updates

## Allow Ping
* `netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=allow`

## Rename Admin Account
* `wmic useraccount where name='Administrator' rename SomethingElseAdmin`

## Update Path
* Add c:\bin to the System Variable via regedit:
* HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment\Path

## Temp Folder
* `mkdir c:\temp`

## Install Antivirus
* Reboot after installation

## AD Protection
In AD, move the new VM to the appropriate OU under Servers, and the under its object tab, click Protect from Accidental Deletion

## Task Manager
* Add Disk to Task Manager -> Performance
* `diskperf -Y`
* (then restart taskmgr)

## WSUS Key Fix - only needed if VM is deployed from a Template
```
net stop wuauserv
REG DELETE "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate" /v AccountDomainSid /f
REG DELETE "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate" /v PingID /f
REG DELETE "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate" /v SusClientId /f
REG DELETE "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate" /v SusClientIdValidation /f
wuauclt /resetauthorization /detectnow
```

## Backups
* Add the VM to your backup rotation.

## Change RDP Port - regedit
* Do not expose the RDP port to the Internet!
* HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Rcp
* portNumber = 54321

## Change RDP Port - command line
* Do not expose the RDP port to the Internet!
* netsh advfirewall firewall add rule name="54321 Remote Desktop" dir=in action=allow protocol=TCP localport=54321
* (in sconfig, you will now need to Enable Remote Desktop)


___


## Miscellaneous Config

___

### Powershell: Add User to Login As a Batch Job Security Rights (this is for Scheduled Tasks)
* https://gallery.technet.microsoft.com/scriptcenter/Powershell-Add-User-to-fcf4adff

### Create a Network Share Example
* `net share uhs="c:\test" /grant:"test-ad\Information Technology",CHANGE /users:100 /cache:None`
* `icacls c:\test\* /grant:r "test-ad\Information Technology":(OI)(CI)M /T`

### Add a local user example
* `net user colposcope *  /add  /ACTIVE:yes  /EXPIRES:never /usercomment:"Used by Womens Clinic Colposcope"`
* `net user colposcope /expires:never`
* `wmic USERACCOUNT WHERE "Name='colposcope'" set PasswordExpires=False`

### Scheduled Tasks Example
* `schtasks /create /tn "Move SFTP Files" /tr "C:\test\scheduled_task.bat" /sc daily /st 23:59 /RU SYSTEM`
* (This runs a task at 11:59 PM each day with the SYSTEM account)

* You can create an XML file for a more fine-grained task:
* [https://stackoverflow.com/questions/1020023/specifying-start-in-directory-in-schtasks-command-in-windows](https://stackoverflow.com/questions/1020023/specifying-start-in-directory-in-schtasks-command-in-windows)

### Server Core - GUI Programs
* It is also handy to have these programs installed on Server Core
* [https://www.nirsoft.net/utils/task_scheduler_view.html](https://www.nirsoft.net/utils/task_scheduler_view.html)
* [https://www.nirsoft.net/utils/full_event_log_view.html](https://www.nirsoft.net/utils/full_event_log_view.html)

