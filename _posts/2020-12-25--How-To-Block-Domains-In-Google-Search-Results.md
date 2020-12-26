
---
layout: post
title: "How To Block Domains In Google Search Results"
date: 2020-12-25 23:59:32 +0000
categories: google uBlock
tags: google uBlock
excerpt: Configure uBlock Origin to block search results for a given domain
---

I ran across a Hacker News thread asking [Why does Pinterest dominate Google text search results?](https://news.ycombinator.com/item?id=25538586)
and really enjoyed some of the comments.  User `ffpip` posted a nice way to block all `Pinterest` search results when using `uBlock Origin`.

This was done by adding the following to the `My filters` section:

    google.*##.g:has(a[href*=".pinterest."])
    google.*##a[href*=".pinterest."]:nth-ancestor(1)

I want to remember this technique for when I want to block annoying search reseach from a domain that I will never visit.

