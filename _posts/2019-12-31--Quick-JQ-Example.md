---
layout: post
title: "Quick JQ Example"
date: 2019-12-31 15:00:10 +0000
categories: batchfile json jq
---


## [Quick jq example](https://gist.github.com/jftuga/e72c5089c060586d1b23041ca6a3870c)

**File:** getNames.bat

```
@echo off
rem use -r to return strings without surrounding double-quotes
jq -r ".Buckets | .[] | .Name" s3.json

```

---


**File:** s3.json

```
{
    "Buckets": [
        {
            "Name": "examplelogs",
            "CreationDate": "2018-07-11T15:35:14.000Z"
        },
        {
            "Name": "example.net",
            "CreationDate": "2019-11-17T11:36:46.000Z"
        },
        {
            "Name": "s3.example.net",
            "CreationDate": "2018-07-17T18:16:37.000Z"
        },
        {
            "Name": "example2.io",
            "CreationDate": "2019-11-12T15:46:34.000Z"
        },
        {
            "Name": "example3.com",
            "CreationDate": "2018-07-18T02:33:46.000Z"
        },
        {
            "Name": "www.example.net",
            "CreationDate": "2019-11-17T11:35:25.000Z"
        },
        {
            "Name": "www.example2.io",
            "CreationDate": "2019-11-12T15:43:29.000Z"
        },
        {
            "Name": "www.example3.com",
            "CreationDate": "2018-07-18T02:34:20.000Z"
        }
    ],
    "Owner": {
        "DisplayName": "example",
        "ID": "ooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo"
    }
}


```


