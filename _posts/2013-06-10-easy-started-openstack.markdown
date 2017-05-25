---
layout: post
title:  "The Easy Way To Get Started With OpenStack"
redirect_from:
   - /easy-started-openstack
date:   2013-06-10 00:08:47 +0100
categories: best domain registrar
description: I have recently been reading and learning a whole lot about more about linux, networking and specifically OpenStack. I have us
---

I have recently been reading and learning a whole lot about more about linux, networking and specifically [OpenStack](http://www.openstack.org/ "OpenStack"). I have used CentOS for web servers before and with the help of Google I have no problem using the command line to perform tasks with a pretty good understanding of how things work and how to keep the web server protected. But recently I have gotten interested in [OpenStack](http://www.openstack.org/ "OpenStack") and I feel like a total beginner when it comes to configuring and getting things setup, however I recently stumbled across something which should have been obvious when I first started. So here is my guide to get started with [OpenStack](http://www.openstack.org/ "OpenStack")which only requires three steps and can get you up and running in less than 30 minutes. Step 1: Install [Ubuntu](http://www.ubuntu.com/download "Ubuntu Download"), you can pick either the desktop version or the server version, it is up to you. Step 2: Get git

> `sudo apt-get install git`

 Step 4: Run the devstack script to install the components of [OpenStack](http://www.openstack.org/ "OpenStack") This can be done with the following command in terminal > `git clone git://github.com/openstack-dev/devstack.git`

 Then once its downloaded use the following command in terminal > `cd devstack; ./stack.sh`

 Then you are done, go to your local ip in your web browser to login to your OpenStack control panel. I stumbled upon the scripted install guide [here](http://docs.openstack.org/trunk/openstack-compute/admin/content/scripted-ubuntu-installation.html "Scripted Install Ubuntu").