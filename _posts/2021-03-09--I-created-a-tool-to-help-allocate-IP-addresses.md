---
layout: post
title: "I created a tool to help allocate IP addresses"
date: 2021-03-09 23:37:21 +0000
categories: sysadmin networking golang
tags: sysadmin networking golang
excerpt: Allocating IP addresses
---

**Synopsis**

I wrote a command line program to help find unused IP addresses.  This will be useful when assigning a new IP address to a VM, printer, network device, etc. For any devices on our network that gets a static IP address reservation, our Active Directory DNS is the authoritative source along with a spreadsheet containing network CIDRs / VLAN designations. For example, security cameras have their own network block / VLAN.  

**Scenario**

A new VM needs to be created.  The first step is to examine our spreadsheet for the correct CIDR block.  Next, we would traditionally then open the Windows DNS mmc and look for a free IP address in the appropriate range.  *One of the inconveniences with this process is that Windows DNS sorts IP addresses lexigraphically instead of numerically thus making it more difficult to find a good, unused IP address.*  The program I wrote partially automates this process.

**Example**

An IP address for a VM needs to be created in this network: 192.168.10.**64**/26.  You want to give the program the first IP address to check which is typically one plus the CIDR. In this case 192.168.10.**65**/26.

Therefore, I can run:

    C:\> nextfreeip.exe 192.168.10.65/26

    192.168.10.65     server1.example.com
    192.168.10.66     server2.example.com
    192.168.10.67     server3.example.com

    192.168.10.68 is not in DNS

I now know what the first available address is.  (The program will stop searching after the last IP address in the given CIDR block is queried in DNS.)  To to make sure it is unused, I will trying pinging it and running a port scan on the address before statically assigning it (in DHCP/DNS) to a VM, copier, network device, etc. 

**Download**

https://github.com/jftuga/nextfreeip

Binaries for Windows, macOS and Linux can be found on the [releases](https://github.com/jftuga/nextfreeip/releases) page -- or you can compile from *golang* source.

