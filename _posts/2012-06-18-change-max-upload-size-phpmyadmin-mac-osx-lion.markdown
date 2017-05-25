---
layout: post
title:  "Change Max Upload Size in PHPMyAdmin on Mac OSX Lion"
redirect_from:
   - /change-max-upload-size-phpmyadmin-mac-osx-lion
date:   2012-06-18 00:01:53 +0100
categories: best domain registrar
description: I'm constantly installing and reinstalling phpmyadmin on my macs and I keep having to look up how to change the max upload size. It's fairly simple actually, you simply need to edit the following thin
---

I'm constantly installing and reinstalling phpmyadmin on my macs and I keep having to look up how to change the max upload size. It's fairly simple actually, you simply need to edit the following things in your php.ini file. My php.ini file was located in my /etc/ folder. upload\_max\_filesize memory\_limit post\_max\_size Set these to whatever you want the limit at, I set mine high since its on my local computer, so 128M. Then use the following command in your terminal to restart apache on Mac OSX Lion. sudo /usr/sbin/apachectl restart