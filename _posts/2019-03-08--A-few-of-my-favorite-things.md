---
layout: post
title: "A few of my favorite things"
date: 2019-03-08 17:33:36 +0000
categories: sysadmin
---

I'd like to share with you some of my command-line tools that I have written to make my job easier.  These are cross-platform (Windows, Linux, MacOS). I have also provided Windows binaries (usually found in a releases section).  Most of my programs are either written in Python 3.5 or Go. My over-arching goal is to provide a self-contained, single binary or single .py file to maximize usability.

___

[tcpscan](https://github.com/jftuga/tcpscan)

* TCP Scan
* A fast, simple, multi-threaded, cross-platform, IPv4 TCP port scanner

This is similar to nmap, but not nearly as feature rich.  What I like about it is that it is just one file that I
can use on any OS.  It has a few features that nmap does not.  If I want to scan all ports, I can use `-p all` instead of `-p 1-65535`.  The `-lo` option will continually scan a port every second until it is open. Once opened, it will ring an audible bell and stop scanning.  This is nice if I have to reboot a device and want to know when the service is back up and running.  I can be working in a different window and just listen for the bell.  The `-lc` option is similar, but it scans an open port and rings a bell once the port is closed.  The last unique feature it has is `-L`. In this mode, it can listen to a group of TCP ports and let you know when and what IP address has connected to it.

___

[duu](https://github.com/jftuga/duu)

* Disk Usage Utility
* Recursively display disk usage in kilobytes of the given directory.

This is similar to `du` but has some unique features.  The summary displays the the number of files, sizes in kilobytes, megabytes, and (if large enough) gigabytes.  It supports filtering with regular expressions.  With `-e`, it can also report the number of files per extension. With `-n`, it will skip directories that start with a dot.  I find this useful for skipping `.git` folders.  The best option of this program is `-T`.  This tells the program how many threads to use and can really speed up the program when traversing a SAN.

___

[freq](https://github.com/jftuga/freq)

* Frequency of data
* Display the frequency of each line in a file or from STDIN

This is similar to running `(do something) | sort | uniq -c | sort -nr`, but usually faster and uses less memory.  It has some unique command line options including the ability to resolve IP addresses when using the `-d` switch.

___

[dcmp](https://github.com/jftuga/dcmp-py)

* Directory Compare
* Compare files within two directory trees for equivalency

This is *somewhat* similar to `diff` but is more focused on comparing metadata.  The output is easier to read as it displays differences in an easy to read ASCII table.  When comparing two directory trees, it tells you differences in file sizes and file modification times.  It can also tell you which files are in one directory but not the other.  With 
`-T` it will use multiple threads which is especially useful in conjunction with `-e`.  This compares file contents as well as metadata.

___

[ipinfo](https://github.com/jftuga/ipinfo)

* IP Info
* Return IP address info including geographic location and distance when given IP address, email address, host name or URL

This is like an enhanced version of `nslookup`. For a given input, it returns hostname, ASN org, city, region(state), country, and computes the distance from your current IP address. This was my first program that I wrote in `Go`. It uses the https://ipinfo.io web site to retrieve IP information.

___

[timeshift](https://github.com/jftuga/timeshift)

* Time Shift
* Shift date/time in log files or from STDIN

Every now and then I need to read log files that were saved in UTC which can be annoying to convert to local time. I wrote a `Go` program that can shift a log file date/time stamp by days, hours, minutes, and seconds. Other than shifting time, the program can also be used to simply change date/time formats.

___

[fstat](https://github.com/jftuga/fstat)

* File Stat
* Get info for a list of files across multiple directories

This is similar to running `ls -l` or `dir` across multiple directories, but with a much nicer output format. Sometimes I need to find a file by filename but am not sure where is is located.  I can pipe the output of `find` or `dir /s/b` to this program and get a group of file sizes and modification times.  There are a lot of sorting options, too.  The examples on the repo page make it more clear how this works.

___

[compinfo](https://github.com/jftuga/compinfo)

* Comp Info
* Display basic computer info

This is a Windows GUI program to display basic computer info all in one place.  In a single window, it displays: computer name, OS version, computer model & serial number, CPU & number of cores, memory, graphics, disks, IPv4, MAC, interface speed, and uptime. This was one of my first programs that I wrote using C#, WPF and XAML.  Developing this program (especially the XAML portion) in Visual Studio was a great experience.  The git repo has a screenshot of how this operates. This is installed on our Windows 10 base MDT image.

___

[WinLLDPService](https://github.com/jftuga/WinLLDPService)

* Win LLDP Service

This is a LLDP 'client' (written in C#) that runs as a Windows service. I forked this from another github repo.  By running a *show LLDP port info* style command on a network switch port, you can get computer name, service tag, logged in user, uptime, OS, gateway, netmask and DHCP server.  This is installed on our Windows 10 base MDT image.

___

[spo_view_it](https://github.com/jftuga/spo_view_it)

* SharePoint Online View It
* Chrome extension

Sometimes SharePoint Online defaults to opening a Word or Excel document in the Editing View instead of the preferred Reading View. This extension changes the URL so that the Reading View is always opened. You will still be able to edit pages by clicking: Edit Document -> Edit in Browser. We have deployed this to about 250 desktops via Group Policy.  Over 1300 people are using this - yay!

___

[flash_read_cache_stats](https://github.com/jftuga/flash_read_cache_stats)

* View stats from VMWare ESX's Flash Read Cache using Grafana and InfluxDB

This is a method to save (long-term) and view statistics for the vSphere Flash Read Cache. It was tested against ESX 6.7. See the example graphs at the bottom of the repo page.


