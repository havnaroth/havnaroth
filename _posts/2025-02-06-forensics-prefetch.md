---
layout: post
title:  "Windows Forensics 101 - Prefetch"
date:   2025-02-06 17:40:00 +0100
categories: ["Windows Forensics 101"]
tags : [forensics]
---

Windows Prefetch 

Prefetch is stored in location `C:\Windows\Prefetch` as *.pf files. 

Those files serve performence improvement of applications and are created upon executing binary. 
This artifact is not present on Windows Server hosts by default however that setting can be altered with registry change under following path.

```text
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters
```

![img-description](/assets/img/windows-regedit-prefetch.png)
_Regedit_

Under that path following values can be set 

- `EnablePrefetcher` - 0, 1, 2
- `EnableSuperfetch` - 0, 1, 2

Where `EnablePrefetcher` values mean 

- 0 - Disabled
- 1 - Application Launch Prefetch
- 2 - Boot Prefetch

And `EnableSuperfetch` values mean 

- 0 - Disabled
- 1 - Application Launch Prefetch
- 2 - Boot Prefetch

To analyze prefetch files we can utilize one of Eric Zimmermann tools, but also surely big parsers like Belkasoft, FTK, Encase or Autopsy can do it for us.
For EZ its tool PECmd - <https://ericzimmerman.github.io/#!index.md>

PECmd as all EZ tools have help section that can be easily accessed with `-h` argument.

![img-description](/assets/img/windows-prefetch-pecmd.png)
_PECmd_

```shell
C:\Users\Hannon>C:\Users\Hannon\Desktop\PECmd\PECmd.exe -f C:\Windows\Prefetch\ANYDESK.EXE-D1F901EB.pf
PECmd version 1.5.1.0

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/PECmd

Command line: -f C:\Windows\Prefetch\ANYDESK.EXE-D1F901EB.pf

Warning: Administrator privileges not found!

Keywords: temp, tmp

Processing C:\Windows\Prefetch\ANYDESK.EXE-D1F901EB.pf

Created on: 2025-01-07 11:11:47
Modified on: 2025-01-07 13:13:15
Last accessed on: 2025-02-06 21:20:51

Executable name: ANYDESK.EXE
Hash: D1F901EB
File size (bytes): 86 876
Version: null

Run count: 4
Last run: 2025-01-07 13:13:05
Other run times: 2025-01-07 11:11:38, 2025-01-07 11:11:38, 2025-01-07 11:11:37

Volume information:

#0: Name: \VOLUME{01db0e69bd6571c7-92bd7b09} Serial: 92BD7B09 Created: 2024-09-24 10:08:44 Directories: 43 File references: 183

Directories referenced: 43

(...)
Files referenced: 134
(...)
15: \VOLUME{01db0e69bd6571c7-92bd7b09}\USERS\HANNON\DOWNLOADS\ANYDESK.EXE (Executable: True)
(...)

---------- Processed C:\Windows\Prefetch\ANYDESK.EXE-D1F901EB.pf in 0,14420140 seconds ----------
```

For the sake of clarity i've removed some irrelevant data from output, but what we can clearly observe is following informations
- What file was processed (pf file)
- When record was created - and we can associate that entry with first execution of the binary in the OS (assuming pf filese were not removed previously)
- When was the last time when binary was executed 
- How many times binary was executed (with last 3 times) 
- And from referenced directories we can find the path with our binary from where it was executed. 