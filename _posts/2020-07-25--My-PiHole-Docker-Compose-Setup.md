---
layout: post
title: "My PiHole Docker Compose Setup"
date: 2020-07-25 16:44:27 +0000
category: 2020
tags: pihole docker
excerpt: A method to version your docker containers and revert to older one if needed
---

I prefer to use *versioned* docker images instead of always pulling from `latest`.  This enables hassle free upgrades: if you run a new image version and it's buggy, then it's very easy to revert back to a known good version.

___

I have modified the default PiHole [docker-compose.yml](https://github.com/pi-hole/docker-pi-hole) file.

In my file, I have removed the DHCP and HTTPS sections:

* 67:67/udp
* 443:443/tcp
* cap_add: NET_ADMIN

I have also modified:

* TZ
* added a `/usr/local/bin` volume (mounted to `/opt/bin` - just for my personal use)
* uncommented the `./var-log/pihole.log` volume

___

At the time of this writing I am using the `v5.1.1` image instead of `latest` even though they point to the same repository.

The reason that I went with the `v5.1.1` tag is that it will be easier to revert in the future if needed. For example, if `v5.1.2` *(a future version)* is buggy I will be able to revert whereas if I was only using `latest` it would not be as easy to do so.  

___

For upgrades on my *Raspberry Pi*, I have the following directories:

* v5.1.1 *(running version)*
* v5.1.2 *(newly released version)*

Each directory will contain their own `docker-compose.yml` file.  I can then use the PiHole `teleporter` to export settings from the old container and then import those same settings into the new one.  I don't mind manually reconfiguring any other settings that teleporter does not catch.

___

I have also successfully used a similar procedure for my [Unifi Controller](https://github.com/ryansch/docker-unifi-rpi) containers. It also has the capability to export and import settings which makes it an ideal candidate for this specific method of Docker containerization.
