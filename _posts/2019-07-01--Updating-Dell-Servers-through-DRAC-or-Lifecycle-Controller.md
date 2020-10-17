---
layout: post
title: "Updating Dell Servers through DRAC or Lifecycle Controller"
date: 2019-07-01
categories: hardware sysadmin
tags: hardware sysadmin
excerpt: A straight-forward recipe on how to quickly update all Dell server firmwares.
---

## Updating through DRAC
* Maintenance -> System Update 
* HTTPS address: 143.166.147.76 (this is the IP address for ftp.dell.com but using the IP address seems to work better)
* Scroll to bottom -> click Check for update
* A list of what needs to be updated will appear
* Select what you want and then click either install & reboot or install at next reboot

## RAID configuration through DRAC
* Configuration -> Storage Config
* Scroll down -> Virtual Disk Config

## Updating the Lifecycle Controller
* From DRAC boot into the Lifecycle Controller  or press F10 when booting
* Settings -> Network
* Firmware Updates -> Launch Firmware Update and/or Rollback
* FTP: 143.166.147.76  (skip username / password)
* OS Deployment will install Dell hardware specific drivers

## Updating via bootable Linux ISO

* Dell Tech support does not recommend using the Linux ISO for updates as it is not updated as frequently and (apparently) does not work very well.
* [Dell Server Software Repository](https://www.dell.com/support/article/en-us/sln296511/update-poweredge-servers-with-platform-specific-bootable-iso?lang=en)
