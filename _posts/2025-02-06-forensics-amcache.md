---
layout: post
title:  "Windows Forensics 101 - Amcache"
date:   2025-02-07 11:58:00 +0100
categories: ["Windows Forensics 101"]
tags : [forensics]
---

Windows Amcache 

## What is Amcache?

Amcache is a registry hive file in Windows operating systems that stores information about programs that have been executed on the system. It is primarily used for application compatibility purposes, but it can also be a valuable source of forensic evidence. The Amcache file contains details such as the file path, execution time, and other metadata about the executed programs, which can help investigators understand user activity and identify potentially malicious software.

Amcache is located under path 

```text
C:\Windows\AppCompat\Programs\Amcache.hve
```

It can be parsed among the others with EZ tool called [**AmcacheParser**][amcacheparser]

![img-description](/assets/img/windows-amcacheparser.png)
_AmcacheParser from EZ_




So since we know what we need to analyze and how to analyze it we can proceed. 

```shell
C:\Users\Hannon>C:\Users\Hannon\Desktop\AmcacheParser\AmcacheParser.exe -f C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve --csv C:\Users\Hannon\Desktop\AmcacheParser
```

```shell
C:\Users\Hannon>C:\Users\Hannon\Desktop\AmcacheParser\AmcacheParser.exe -f C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve --csv C:\Users\Hannon\Desktop\AmcacheParser
AmcacheParser version 1.5.2.0

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/AmcacheParser

Command line: -f C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve --csv C:\Users\Hannon\Desktop\AmcacheParser

Warning: Administrator privileges not found!

Two transaction logs found. Determining primary log...
Primary log: C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve.LOG1, secondary log: C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve.LOG2
Replaying log file: C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve.LOG1
Replaying log file: C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve.LOG2
At least one transaction log was applied. Sequence numbers have been updated to 0x183C9. New Checksum: 0xE6D83FDB
Two transaction logs found. Determining primary log...
Primary log: C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve.LOG1, secondary log: C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve.LOG2
Replaying log file: C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve.LOG1
Replaying log file: C:\Users\Hannon\Desktop\AmcacheParser\Amcache.hve.LOG2
At least one transaction log was applied. Sequence numbers have been updated to 0x183C9. New Checksum: 0xE6D83FDB
Error parsing ProgramsEntry at {11517B7C-E79D-4e20-961B-75A811715ADD}\Root\InventoryApplication\0000855869b15cec4941316273d2cb47d14f00000904. Error: String '20/24/2024 00:00:00' was not recognized as a valid DateTime.
System.FormatException: String '20/24/2024 00:00:00' was not recognized as a valid DateTime.
   at System.DateTimeParse.Parse(ReadOnlySpan`1 s, DateTimeFormatInfo dtfi, DateTimeStyles styles)
   at Amcache.AmcacheNew..ctor(String hive, Boolean recoverDeleted, Boolean noLogs)
Please send the following text to saericzimmerman@gmail.com
Key data: Key Name: 0000855869b15cec4941316273d2cb47d14f00000904
Key Path: {11517B7C-E79D-4e20-961B-75A811715ADD}\Root\InventoryApplication\0000855869b15cec4941316273d2cb47d14f00000904

Last Write Time: 06.02.2025 09:25:06 +00:00
(...)
```

The output is way longer but we dont need to see it all as the essence is stored in CSV files. 

![img-description](/assets/img/windows-amcache-yield.png)
_AmcacheParser yield_

Usually the thing thats most interesting is `20250207120402_Amcache_UnassociatedFileEntries.csv` however this may depend on kind of conducted investigation.

So we open that up with TimelineExplorer for nice clear view. 

![img-description](/assets/img/windows-amcache-timeline.png)
_AmcacheParser results opened in TimelineExplorer_

And from that view we can learn what binaries were executed, when were they executed and SHA1 of that file. 
What is important here - is the fact that provided SHA1 value is calulated for only first 31,457,280 bytes of binary.
So if file is larger then this hash may provide inaccurate result. 

[amcacheparser]: <https://ericzimmerman.github.io/#!index.md>