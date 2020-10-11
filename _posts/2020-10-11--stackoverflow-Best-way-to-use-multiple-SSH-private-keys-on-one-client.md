---
layout: post
title: "stackoverflow: Best way to use multiple SSH private keys on one client"
date: 2020-10-11 16:38:40 +0000
categories: ssh ssh-keys openssh
---

[stackoverflow: Best way to use multiple SSH private keys on one client](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client)

Question Date: 2010-03-10 13:40:58

I want to use multiple private keys to connect to different servers or different portions of the same server (my uses are system administration of server, administration of Git, and normal Git usage within the same server). I tried simply stacking the keys in the `id_rsa` files to no avail.

Apparently a straightforward way to do this is to use the command 
      
    ssh -i <key location> login@server.example.com 

That is quite cumbersome.

Any suggestions as to how to go about doing this a bit easier?


