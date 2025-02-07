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

It can be parsed among the others with EZ tool called [**AmcacheParser **][parser]

![img-description](/assets/img/windows-amcacheparser.png)
_AmcacheParser from EZ_


[amcacheparser]: <https://havnaroth.github.io/posts/forensics-ual>