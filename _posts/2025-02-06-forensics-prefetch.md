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
This artifact is not present on Windows Server hosts by default however that setting can be altered with registry change under following path

```text
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters
```

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


![img-description](/assets/img/windows-regedit-prefetch.png)
_Regedit_


