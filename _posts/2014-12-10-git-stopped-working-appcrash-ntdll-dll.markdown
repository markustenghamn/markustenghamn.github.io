---
layout: post
title:  "Git Has Stopped Working [APPCRASH, ntdll.dll]"
redirect_from:
   - /git-stopped-working-appcrash-ntdll-dll
date:   2014-12-10 22:45:11 +0100
categories: best domain registrar
description: I am leaving this mostly as a not for myself and in the hope that it may help someone. If you have recently installed or updated Airfoil then this...
---

I am leaving this mostly as a not for myself and in the hope that it may help someone. If you have recently installed or updated Airfoil then this may be the cause of your issue as described [here](http://stackoverflow.com/questions/24734369/git-for-windows-stopped-working "Git For Windows Stopped Working"). You could try uninstalling or disabling the Instant On feature of airfoil but if it does not work you can edit the registry directly. By removing `AirfoilInject3.dll` from `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Windows\AppInit_DLLs` using regedit you can solve this issue. This issue has happened to me twice and it has always been a problem with Airfoil. It could also be another program causing this issue such as your antivirus program which you can try disabling temporarily.