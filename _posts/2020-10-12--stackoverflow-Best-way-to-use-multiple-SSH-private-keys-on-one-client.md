---
layout: post
title: "stackoverflow: Best way to use multiple SSH private keys on one client"
date: 2020-10-12 19:26:31 +0000
categories: openssh ssh ssh-keys
tags: openssh ssh ssh-keys
excerpt: How to efficiently connect to multiple systems via SSH using key authentication
---

## Intro

This is used by `VS Code` when connecting to a remote server via SSH.  It is also used by the `Windows 10` version of `ssh.exe`

___


## [stackoverflow: Best way to use multiple SSH private keys on one client](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client)

Question Date: 2010-03-10 13:40:58

I want to use multiple private keys to connect to different servers or different portions of the same server (my uses are system administration of server, administration of Git, and normal Git usage within the same server). I tried simply stacking the keys in the `id_rsa` files to no avail.

Apparently a straightforward way to do this is to use the command 
      
    ssh -i <key location> login@server.example.com 

That is quite cumbersome.

Any suggestions as to how to go about doing this a bit easier?


----

## [answer](https://stackoverflow.com/a/2419609/452281)


Answer Date: 2010-03-10 13:46:39

From my `.ssh/config`:

    Host myshortname realname.example.com
        HostName realname.example.com
        IdentityFile ~/.ssh/realname_rsa # private key for realname
        User remoteusername

    Host myother realname2.example.org
        HostName realname2.example.org
        IdentityFile ~/.ssh/realname2_rsa  # different private key for realname2
        User remoteusername

Then you can use the following to connect:

`ssh myshortname`

`ssh myother`

And so on.

