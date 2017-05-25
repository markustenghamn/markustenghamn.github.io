---
layout: post
title:  "Upgrade rsyslog on Centos to version 8"
redirect_from:
   - /upgrade-rsyslog-centos-version-8
date:   2016-06-08 20:15:41 +0100
categories: best domain registrar
description: .
---

So for some reason you would like to upgrade rsyslog to version 8. For me it was some issues with the logging, I wasn't getting anywhere debugging version 5.8 and almost nothing was writing to /var/log/messages which is not good. So I decided to figure out how to make the upgrade. First thing you need to do is to ssh to the server or if you have physical access just open a terminal and cd to the repo directory with "cd /etc/yum.repos.d/". Now me need to get the latest repository version which can be found here <http://rpms.adiscon.com/> Version 8 is the latest version while I write this so I simply download the repo using wget. Like so "sudo wget http://rpms.adiscon.com/v8-stable/rsyslog.repo" Then you can proceed to update rsyslog using "yum install rsyslog". Now you are done, you should be running the latest version of rsyslog! This was quick and simple and worked flawlessly for me but I know that it may not work so well for everyone. While searching I had this guide to help me from the rsyslog website which also helps with debugging: <http://www.rsyslog.com/rhelcentos-rpms/>. It can also be a very good idea to check that what you downloaded was actually what you wanted, the link above explains this part in more detail as well. As always, be careful when making changes to your system, make a backup and read things 3 times before trying them, something could go wrong very easily if you don't know what you are doing. Hope the guide helped, thanks for reading!