---
layout: post
title: "PowerShell Test-is64bit"
date: 2018-02-05 04:28:14 +0000
categories: 2018
tags: powershell
excerpt: A PS function to determine if an executable is 32 or 64 bit
---


## gist: [PowerShell Test-is64bit](https://gist.github.com/jftuga/97447fef14969cfe2a91988d953e26c0)

**File:** Test-is64bit.ps1

```
function Test-is64Bit {
    param($FilePath)

    [int32]$MACHINE_OFFSET = 4
    [int32]$PE_POINTER_OFFSET = 60

    [byte[]]$data = New-Object -TypeName System.Byte[] -ArgumentList 4096
    $stream = New-Object -TypeName System.IO.FileStream -ArgumentList ($FilePath, 'Open', 'Read')
    $stream.Read($data, 0, 4096) | Out-Null

    [int32]$PE_HEADER_ADDR = [System.BitConverter]::ToInt32($data, $PE_POINTER_OFFSET)
    [int32]$machineUint = [System.BitConverter]::ToUInt16($data, $PE_HEADER_ADDR + $MACHINE_OFFSET)

    $result = "" | select FilePath, FileType, Is64Bit
    $result.FilePath = $FilePath
    $result.Is64Bit = $false

    switch ($machineUint) 
    {
        0      { $result.FileType = 'Native' }
        0x014c { $result.FileType = 'x86' }
        0x0200 { $result.FileType = 'Itanium' }
        0x8664 { $result.FileType = 'x64'; $result.is64Bit = $true; }
    }

    $result
}

Test-is64bit $args[0]

```

___


To check a group of files:

``
    dir *.exe | foreach { .\Test-is64bit.ps1 $_ } | Sort-Object Is64Bit
``

