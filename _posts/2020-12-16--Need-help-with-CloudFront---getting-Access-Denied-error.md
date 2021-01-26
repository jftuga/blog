---
layout: post
title: "Need help with CloudFront - getting Access Denied error"
date: 2020-12-16 22:14:29 +0000
categories: aws
---

I have a static S3 site that sits behind a CloudFront distribution.  The main web pages work fine, but I get `Access Denied` errors when I attempt to go to:

    https://example.com/folder/

However, this works:

    https://example.com/folder/index.html

After googling, I saw that I might want to edit my `Origin Domain Name` to include the `AWS Region` under `Origin Settings`.

So I changed it from:

    www.example.com.s3.amazonaws.com

to:

    www.example.com.s3-website-us-east-2.amazonaws.com

When I did this, I get a new error:

    Error: Failed to contact the origin.

My S3 bucket lives in `US East (Ohio) us-east-2` and the `Default Root Object` is set to `index.html`.

How can I fix this? 

Thanks.

**EDIT:** This is what I am precisely experiencing: [How Headers Work With Default Root Objects](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DefaultRootObject.html#DefaultRootObjectHow)

___

**EDIT 2:** My **less than optimal** solution

This solution allows for:

* HTTPS website served by only `CloudFront` with no other direct access.
* URLs such as `https://example.com/folder/` will serve the corresponding `index.html` file.

How I did it:

* **CloudFront**
* `Origin Doman Name:` www.example.com.s3-website-us-east-2.amazonaws.com
* `Enable IPv6:`unchecked
* `SSL Certificate:` Custom SSL Certificate - use `ACM` to create the certificate for your S3 hosted web site

* **Route 53**
* Routing Policy: `Simple routing`
* Record name: `www`
* Record Type: `A`
* Route traffic to: `Alias to CloudFront distribution`
* *The CF distribution should be listed in the dropdown*

* **S3**
* `Static website hosting:` Enabled
* `Hosting type:` Host a static website
* `Index document:` index.html
* `Block public access (bucket settings):` Block *all* public access is enabled


**Bucket Policy**

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::www.example.com/*",
                "Condition": {
                    "IpAddress": {
                        "aws:SourceIp": [
                            "THIS NEEDS",
                            "TO BE CHANGED"
                        ]
                    }
                }
            }
        ]
    }

This is how I populated the `SourceIp` with the `CloudFront IP Ranges`:

    curl -s https://ip-ranges.amazonaws.com/ip-ranges.json | jq '.prefixes | .[] |  select( .service == "CLOUDFRONT" ) | .ip_prefix' | paste -sd, -

Since there are no `IPv6` blocks in this list of 120 ranges, I unchecked the `IPv6` option in `CloudFront`. Unfortunately, this list may need to be periodically updated.  This is my temporary work-around until I find a more permanent solution.

See also: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/LocationsOfEdgeServers.html



