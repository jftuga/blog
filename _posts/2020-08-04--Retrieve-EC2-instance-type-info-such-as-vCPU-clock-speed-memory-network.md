---
layout: post
title: "Retrieve EC2 instance type info such as vCPU, clock speed, memory, network"
date: 2020-08-04 01:57:49 +0000
categories: aws
tags: aws
excerpt: Using the AWS CLI and JQ to extract EC2 instance-type meta data
---

I wrote a script to retrieve EC2 instance type info such as vCPU, clock speed, memory, network and save it to a tab-separated file.  It's just a nice wrapper around the `aws` command.

I created this for a friend and thought it might be useful to others.  Since he is using Excel, silly me thought he was using Windows, but alas, he is using MacOS.  Either way, you will need to have `jq` installed.

* Windows: [https://gist.github.com/jftuga/b5a4db00f8de73a501331f7b1c0b7332](https://gist.github.com/jftuga/b5a4db00f8de73a501331f7b1c0b7332)
* Mac: [https://gist.github.com/jftuga/18abbec82dfc1b89c97ff55b782e2c5f](https://gist.github.com/jftuga/18abbec82dfc1b89c97ff55b782e2c5f)

I have also written a cross-platform program to get `spotprice` information which I think is a little easier than using the `aws` CLI. It also runs faster and has a few more features.

* spotprice: [https://github.com/jftuga/spotprice](https://github.com/jftuga/spotprice)
