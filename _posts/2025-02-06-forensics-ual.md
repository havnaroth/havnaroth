---
layout: post
title:  "Windows Forensics 101 - User Access Logs - UAL"
date:   2025-02-06 17:40:00 +0100
categories: ["Windows Forensics 101"]
tags : [forensics,ericzimmerman,ual]
---

Windows User Access Logs (UAL)

Present only on hosts with installed Windows Server 2012+ 

```text
C:\Windows\System32\LogFiles\SUM\Current.mdb
C:\Windows\System32\LogFiles\SUM\SystemIdentity.mdb
C:\Windows\System32\LogFiles\SUM\<GUID>.mdb
```

Files are parsable with <https://github.com/EricZimmerman/Sum> and sometimes need to be fixed with following command 

 
```text
esentutl.exe /p Current.mdb
...and remaining files
```

Parsing using following commandline 

```text
SumECmd.exe -d C:\directory_with_files --csv C:\output_directory
```

After fixing and parsing set of CSV files is generated that can be fed into Timeline Explorer for further review 
<https://ericzimmerman.github.io/#!index.md>