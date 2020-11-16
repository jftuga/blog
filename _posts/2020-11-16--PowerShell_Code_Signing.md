---
layout: post
title: "PowerShell Code Signing"
date: 2020-11-16 19:07:00 +0000
categories: powershell
tags: 
excerpt: A quick note on how to do PowerShell code signing
---


{% highlight powershell %}
Set-AuthenticodeSignature '.\example-script.ps1' @(Get-ChildItem cert:\CurrentUser\My -codesign)[0]
{% endhighlight %}

