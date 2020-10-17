---
layout: post
title: "Chrome Extension to Force SharePoint Online to Reading View"
date: 2018-09-18 21:46:01 +0000
categories: sysadmin
tags: sysadmin
excerpt: An Chrome extension to make SharePoint more user-friendly.  Originally used internally, it is now used daily by over 1,000 people.
---

- [Chrome Store](https://chrome.google.com/webstore/detail/spo-view-it/omljlibfjloccmdmmlpcnlijjneabhjm)
- [GitHub](https://github.com/jftuga/spo_view_it)

Our tenant has an infrequent problem in which clicking on a Word or Excel document in SharePoint Online will open the document in Edit mode instead of Reading View mode.  This is very confusing for our users, especially since it just randomly occurs without rhyme or reason.

I wrote a Chrome Extension that forces the browser to open the document in the Reading View mode.  Editors can then use: *Edit Document -> Edit in Browser* to edit documents.
