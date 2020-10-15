---
layout: post
title: "Improving GNU sort speed"
date: 2018-12-19 04:42:02 +0000
categories: command-line
excerpt: How to better use your hardware to reduce GNU sort times
---

You can drastically improve the speed of GNU sort by using a few of its command line options.


From the man page:

    -S, --buffer-size=SIZE
        use SIZE for main memory buffer
        SIZE may be followed by the following multiplicative suffixes: 
        % 1% of memory, b 1, K 1024 (default), and so on for M, G, T, P

    --parallel=N
        change the number of sorts run concurrently to N

    -T use DIR for temporaries, not $TMPDIR or /tmp;

You will probably also want to use:

*Set LC_ALL=C to get the traditional sort order that uses native byte values.*

____

Example:

    export LC_ALL=C
    sort -S 8G --parallel=4 -T /mnt/fast_ssd/tmp unsorted.txt > sorted.txt

Note, this will not use 8 gigs of memory unless it is needed (e.g. the file is very large).  After sort consumes the given memory limit, it will then start using tmp files to complete the sort.  In this case, it is beneficial to use -T to place sort's tmp files on a fast disk.  Your should also not exceed the number of cores your system has when using --parallel.

____

References:

* [gnu sort - default buffer size](https://stackoverflow.com/questions/37514283/gnu-sort-default-buffer-size)
* [How to sort big files?](https://unix.stackexchange.com/questions/120096/how-to-sort-big-files)

___

[Reddit discussion](https://www.reddit.com/r/commandline/comments/a7hq5n/psa_improving_gnu_sort_speed/)

