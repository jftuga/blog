---
layout: post
title: "How to Either Write to STDOUT or to a Buffer in GoLang"
date: 2019-09-09 09:18:53 +0000
categories: golang
excerpt: Use a single bufio variable to output to either a file or the screen.
---


## gist: [demonstrates how to either write to STDOUT or to a buffer in golang](https://gist.github.com/jftuga/4c8b998ad4c581742f425b92d77765ba)

**File:** bufWrite.go

```
// demonstrates how to either write to STDOUT or to a buffer

package main

import (
        "bufio"
        "fmt"
        "os"
    )

func main() {

    var f *os.File
    var err error
    var count int
    fname := "testfile"

    // change to either true|false
    outputToScreen := false
    if outputToScreen {
        f = os.Stdout
    } else {
        f, err = os.OpenFile(fname,os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
        if err != nil {
            fmt.Println("Error opening file")
            return
        }
    }

    writer := bufio.NewWriter(f)
    count, err = writer.WriteString("alpha\n")
    if err != nil {
        fmt.Println("Error writing to buffer")
        return
    }
    writer.Flush()

    if !outputToScreen {
        fmt.Printf("Wrote %d bytes to file: %s\n", count, fname)
    }
}

```


