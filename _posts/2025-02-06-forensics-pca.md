---
layout: post
title:  "Windows Forensics 101 - PCA"
date:   2025-02-07 11:58:00 +0100
categories: ["Windows Forensics 101"]
tags : [forensics]
---

Windows Program Compatibility Assistant

## What is PCA?

The Program Compatibility Assistant (PCA) is a feature in Windows that helps users resolve compatibility issues with older programs. When an older program is detected, PCA can automatically apply compatibility settings to help the program run correctly. Since Windows 11 22H2 those data are additionally stored by PcaSvc directly in filesystem as textfiles. 

## Where is PCA Stored?

PCA settings and logs are stored in the Windows registry and following log files:

```
c:\Windows\appcompat\pca\PcaGeneralDb0.txt 
c:\Windows\appcompat\pca\PcaGeneralDb1.txt
c:\Windows\appcompat\pca\PcaAppLaunchDic.txt
```
## How to process this data 

This doesnt require additional processing as data is stored in text files. 

## What Information Does PCA Contain?

PcaAppLaunchDic.txt is simplified log storing following informations.
- The name of the application
- The path to the executable
- The date and time when the application was launched


PcaGeneralDb1.txt is a bit more detailed log with additions of records like Amcache identifier that may lead to establishing fe. hash of binary. 

![img-description](/assets/img/windows-pca-notepad.png)
_PCA opened in notepad_

