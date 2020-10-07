---
layout: post
title: "Why I Switched From screen To tmux"
date: 2020-06-01 19:12:18 +0000
categories: commandline tmux linux
---

I have used `screen` since the 90s and these are the features along with being able to use screen key bindings *(ctrl-a as the PREFIX)* is what made me switch to `tmux`.  Please look at the *tutorial* at the top of my [.tmux.conf](https://github.com/jftuga/universe/blob/master/tmux.conf) file to see a few more things it can do.  `screen` may also have these same features, but I wanted to learn `tmux` anyway because it seems to be more popular now.

**Feature 1**

You can split one window into multiple panes. These can be either horizontal or vertical.  If you press `prefix "` to create two horizontal panes, there will be a line to divide them. You can then click on that line to resize the panes (making one larger while the other one becomes smaller). This even works in Putty and iTerm2.

**Feature 2**

Two people can attach to the same session.  I could see this being useful in a training or troubleshooting scenario.

